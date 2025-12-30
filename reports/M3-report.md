# Milestone 3 Report: Code-flow Implementation in Nextcloud

## Overview

Milestone 3 has focused on implementing OAuth2-style code flow authentication for Open Cloud Mesh (OCM) federated sharing in Nextcloud as described at [the open cloud mesh spec](https://datatracker.ietf.org/doc/draft-ietf-ocm-open-cloud-mesh/). This enables bearer token based authentication between federated Nextcloud instances for accessing shared resources.

**Scope notes**: [The work performed is provided as a open pull request](https://github.com/nextcloud/server/pull/57234). Additional work aligning with upstream to get the pull request accepted will be provided as voluntary work.

## Work performed

### Issue #28: Make it possible for storage backend to find newer OCM Providers

Probe standardized .well-known endpoint

1. **Probe .well-known endpoint** (`apps/files_sharing/lib/External/Storage.php`)
   - Add `.well-known/ocm` to remote endpoint discovery checks.

**Key Commits:**
- [fix(ocm): probe standardized .well-known endpoint](https://github.com/nextcloud/server/commit/111fa240bc955ee2c1d491dd12899f8cf3c63a79)

### Issue #30: Implement refresh of tokens and code capability

Implemented a complete OAuth2-like token exchange system:

1. **Token Endpoint** (`apps/dav/lib/Controller/TokenController.php`)
   - New `/api/v1/access-token` endpoint for exchanging refresh tokens for access tokens
   - Supports the `authorization_code` grant type
   - Uses permanent refresh tokens to generate short-lived access tokens
   - Includes signature verification of the requests

2. **OCM Provider Capability Discovery** (`lib/private/OCM/Model/OCMProvider.php`)
   - Added `tokenEndPoint` property to OCM provider model
   - Added `exchange-token` capability
   - Updated OCM discovery service to look for token endpoints in remote providers

**Key Commits:**
- [feat(dav): Add token endpoint to exchange refresh tokens for access tokens](https://github.com/nextcloud/server/pull/57234/changes/9ba1a5d3d57900305a6b3acd389faea91926a8ff)

### Issue #29: Implement bearer auth for accessing shares, both incoming and outgoing

Implemented bearer token authentication for WebDAV access:

1. **Refresh Token Generation** (`apps/federatedfilesharing/lib/FederatedShareProvider.php`)
   - Automatically creates permanent refresh tokens when creating remote shares

2. **Bearer Authentication Backend** (`apps/dav/lib/Connector/Sabre/BearerAuth.php`, `lib/private/User/Session.php`)
   - Extended Sabre's `AbstractBearer` for WebDAV authentication
   - Supports both access tokens (for Bearer auth) and long-lived tokens (for Basic auth)

3. **WebDAV Public Endpoint Integration** (`apps/dav/appinfo/v1/publicwebdav.php`)
   - Enabled accessing federated shares with bearer access tokens

4. **Outgoing Share Token Handling** (`apps/files_sharing/lib/External/Storage.php`, `lib/private/Files/Storage/DAV.php`)
   - When requesting remote shares with bearer auth, exchange refresh token for access token

5. **Incoming Share Token Validation** (`apps/federatedfilesharing/lib/Controller/RequestHandlerController.php`)
   - Recognizes incoming tokens as potential access tokens and validates them

**Key Commits:**
- [feat(dav): Add Bearer auth backend for webdav requests](https://github.com/nextcloud/server/pull/57234/changes/2d6428dc5775a93999245f83850122f35238ed57)
- [feat(dav): New method doTryTokenLogin to allow to try token login with token](https://github.com/nextcloud/server/pull/57234/changes/c9f686079ab3ff750541e130a2fa7d1ecf2dcd6d)
- [feat(files_sharing): When requesting a remote share with bearer auth, get an access token](https://github.com/nextcloud/server/pull/57234/changes/db813875e386f672c7412eb42f94fcbfc3587f39)
- [feat(federatedfilesharing): When a remote requests a share with a token, it may be an access token](https://github.com/nextcloud/server/pull/57234/changes/0459fcaa9061e3b194e0145e76fc2928eb1f242b)
- [feat(federatedfilesharing): Create permanent refresh token when creating a remote share](https://github.com/nextcloud/server/pull/57234/changes/6c8062cf09818c4ca5a20fd2fe17b60d9121c44d)
- [feat: Bearer auth aware Sabre HTTP client](https://github.com/nextcloud/server/pull/57234/changes/1d6401b2f27bd037067a5e1382e6c71c6510c828)

### Issue #7: Add support for multi-protocol sharing in Nextcloud

Updated Cloud Federation API to support the new OCM specification format:

1. **Cloud Federation API Adaptations** (`apps/cloud_federation_api/lib/Controller/RequestHandlerController.php`)
   - Updated share creation endpoint to handle new OCM format
   - Supports protocol-specific parameters in share requests
   - Enhanced CloudFederationShare model with protocol information

2. **Cloud Federation Share Model** (`lib/private/Federation/CloudFederationShare.php`)
   - Enhanced to store protocol-specific metadata
   - Backward compatible with existing share format

**Key Commits:**
- [feat(cloud_federation_api): adapt to new format for share creation](https://github.com/nextcloud/server/pull/57234/changes/0efe763f1dc86502e1c07dc3d1ad7984e0239e22)
- [feat: adapt to guzzle api](https://github.com/nextcloud/server/pull/57234/changes/68dd027ac31f8ddf9710541e197bafe689363d4b)
- [feat(cloud_federation_api): support multi protocol for share creation](https://github.com/nextcloud/server/pull/57234/changes/ee0d1879ad46aa4243a3f867fb77b7489be626fd)

---

**Author:** Enrique Pérez Arnaud <enrique@cazalla.net>
**Implementation Period:** November-December 2025
