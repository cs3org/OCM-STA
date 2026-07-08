# Milestone report: Application Federation (M8)

This report covers the work carried out for Milestone M8 of the OCM-STA project.
The milestone focuses on implementing applications federation in Nextcloud,
following the CERNBox model and targeting in particular Jupyterhub, Nextcloud
Talk, and Office-like web apps. Once completed, it must be possible to validate
Nextcloud to CERNBox application sharing and vice versa.

Milestone M8 is tracked in cs3org/OCM-STA#19 (Implement applications federation
in Nextcloud) and cs3org/OCM-STA#20 (Validate Nextcloud to CERNBox application
sharing and vice versa). The public record for the work includes a Nextcloud
app capable of receiving webapp shares according to the spec, a Nextcloud
Jupyterhub integration capable of sharing instances of jupyter, and the needed
pull requests for Nextcloud core.

The contractual milestone deadline in the upstream milestone list is 2026-06-30.

## Executive summary

Preliminary work for this includes providing Nextcloud core with the ability to
deal (both produce and validate) with HTTP Message Signatures (RFC 9421), and
providing Nextcloud with a compatibilty layer that allows it to participate in
webapp sharing according to the spec. This compatibility layer includes
expanding the use of notifications to allow them to act on in shared apps, and
allowing different cloud federation providers to add different protocols for
the same resourceType without the resourceType being duplicated.

Regarding the Nextcloud webapp receiving app, work for this milestone includes
the development of SUNET/ocmremotewebapp. It also includes development of
SUNET/nextcloud-integration_jupyterhub, a nextcloud app integrating jupyterhub
and ready to share instances of the app with ocmremotewebapp.

## Contractual milestone definition

According to the project milestone list, Milestone M8 is defined as:

Milestone M8: Application Federation
Implement applications federation in Nextcloud, following the CERNBox model and
targeting in particular Jupyterhub, Nextcloud Talk, and Office-like web apps.
Validate Nextcloud to CERNBox application sharing and vice versa
Deadline: June 30, 2026

## Work performed

Links to the public repositories and pull requests described below.

1. Milestone tracker and coordination:

   - [cs3org/OCM-STA#19](https://github.com/cs3org/OCM-STA/issues/19):
     Implement applications federation in Nextcloud.
   - [cs3org/OCM-STA#20](https://github.com/cs3org/OCM-STA/issues/20):
     Validate Nextcloud to CERNBox application sharing and vice versa.

2. HTTP Message Signatures (RFC 9421):

   - [nextcloud/server#60136](https://github.com/nextcloud/server/pull/60136):
     Dual stack http-sig (merged).
   - [nextcloud/documentation#15047](https://github.com/nextcloud/documentation/pull/15047):
     Documentation (merged).

3. Compatibility layer in Nextcloud:

   - [nextcloud/server#61201](https://github.com/nextcloud/server/pull/61201):
     Add OCM Notification Received Event (merged).
   - [nextcloud/server#61126](https://github.com/nextcloud/server/pull/61126):
     Merge resource types by name in discovery (merged).

4. Compatibility layer in CERNBox:

   - [cs3org/reva#5611](https://github.com/cs3org/reva/pull/5611): Reworked
     discovery logic and payload (merged).
   - [cs3org/reva#5664](https://github.com/cs3org/reva/pull/5664): New webapp protocol (merged).
   - [cs3org/reva#5695](https://github.com/cs3org/reva/pull/5695): Normalize remote
     user ids in OCM.
   - [cs3org/reva#5696](https://github.com/cs3org/reva/pull/5696): Resolve single-file
     OCM shares.
   - [cs3org/reva#5697](https://github.com/cs3org/reva/pull/5698): Normalize user ids
     on incoming OCM shares.

5. Nextcloud apps:

   - [SUNET/ocmremotewebapp](https://github.com/SUNET/ocmremotewebapp): app for
     receiving webapp shares.
   - [SUNET/nextcloud-integration_jupyterhub](https://github.com/SUNET/nextcloud-integration_jupyterhub):
     app for integrating and sharing jupyterhub.

## Specification changes

In implementing M8 we improved the specification alot with respect to webapp
sharing. Links to the public repositories and pull requests described below
for information purposes only, as they will be formally reported in M10:

1. Webapp design work:

   - in [cs3org/OCM-API](https//github.com/cs3org/OCM-API/pull/356)

2. Webapp sharing specification changes and integration protocol:

   - in [cs3org/OCM-API](https//github.com/cs3org/OCM-API/pull/368)
   - in [cs3org/OCM-API](https//github.com/cs3org/OCM-API/pull/370)
