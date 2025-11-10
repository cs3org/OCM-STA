# Bearer tokens for webdav

## Making webdav calls

To convince nextcloud to make webdav calls with Bearer tokens we touch the code in 2 sites:
1st we make the Sabre http client to know about Bearer token:

```diff
diff --git a/sabre/dav/lib/DAV/Client.php b/sabre/dav/lib/DAV/Client.php
index 1028a6b9..876900a1 100644
--- a/sabre/dav/lib/DAV/Client.php
+++ b/sabre/dav/lib/DAV/Client.php
@@ -66,6 +66,11 @@ class Client extends HTTP\Client
      */
     const AUTH_NTLM = 4;

+    /**
+     * Bearer authentication.
+     */
+    const AUTH_BEARER = 8;
+
     /**
      * Identity encoding, which basically does not nothing.
      */
@@ -131,6 +136,8 @@ class Client extends HTTP\Client
             $userName = $settings['userName'];
             $password = isset($settings['password']) ? $settings['password'] : '';

+                       $is_bearer = false;
+
             if (isset($settings['authType'])) {
                 $curlType = 0;
                 if ($settings['authType'] & self::AUTH_BASIC) {
@@ -142,12 +149,20 @@ class Client extends HTTP\Client
                 if ($settings['authType'] & self::AUTH_NTLM) {
                     $curlType |= CURLAUTH_NTLM;
                 }
+                if ($settings['authType'] & self::AUTH_BEARER) {
+                                       $curlType |= CURLAUTH_BEARER;
+                                       $is_bearer = true;
+                }
             } else {
                 $curlType = CURLAUTH_BASIC | CURLAUTH_DIGEST;
             }

             $this->addCurlSetting(CURLOPT_HTTPAUTH, $curlType);
-            $this->addCurlSetting(CURLOPT_USERPWD, $userName.':'.$password);
+                       if ($is_bearer) {
+                               $this->addCurlSetting(CURLOPT_XOAUTH2_BEARER, $userName);
+                       } else {
+                               $this->addCurlSetting(CURLOPT_USERPWD, $userName.':'.$password);
+                       }
         }
```

This is using the $userName as Bearer token, whose value is generated in `server/apps/federatedfilesharing/lib/TokenHandler.php`:
```php
	public function generateToken() {
		$token = $this->secureRandom->generate(
			self::TOKEN_LENGTH,
			ISecureRandom::CHAR_ALPHANUMERIC);
		return $token;
	}

```
Here TOKEN_LENGTH is 15.

This token is not good for a Bearer token as we see below.
The `generateToken` method is called from `server/apps/federatedfilesharing/lib/FederatedShareProvider.php` in the method `createFederatedShare`.

Then we modify `apps/files_sharing/lib/External/Storage.php`:
```diff
diff --git a/apps/files_sharing/lib/External/Storage.php b/apps/files_sharing/lib/External/Storage.php
index a9781b91a6c..2740a37e2e9 100644
--- a/apps/files_sharing/lib/External/Storage.php
+++ b/apps/files_sharing/lib/External/Storage.php
@@ -92,7 +92,7 @@ class Storage extends DAV implements ISharedStorage, IDisableEncryptionStorage,
                                'host' => $host,
                                'root' => $webDavEndpoint,
                                'user' => $options['token'],
-                               'authType' => \Sabre\DAV\Client::AUTH_BASIC,
+                               'authType' => \Sabre\DAV\Client::AUTH_BEARER,
                                'password' => (string)$options['password']
                        ]
                );
```

## Receiving webdav calls

When the server receives a webdav request with Bearer token, it apparently knows how to find and validate the token.

Up till now, the token has been generated and is stored in the tables `oc_shares` for the sharing side (`token` column) and in `c_remote_shares` for the receiving side (column `share_token`).

But the validation code checks the token against the contents of the `oc_authtoken` table. These tokens are managed by the provider `server/lib/private/Authentication/Token/PublicKeyTokenProvider.php`. This provider also has a `generateToken` method, that will create an entry in the `oc_authtoken`.

## Plan

Make the `createFederatedShare` method in `server/apps/federatedfilesharing/lib/FederatedShareProvider.php` call the `generateToken` method from `PublicKeyTokenProvider.php` instead of from `TokenHandler.php`.

Note:
- I am assuming that the implementation of Bearer token validation already in nextcloud is fine for our purposes
- The proposed implementation willl not result in a short lived access token, it's lifetime would be that of the share
- There is also an OAUTH builtin app which has a table `oc_oauth2_access_tokens` - perhaps we should use that?
