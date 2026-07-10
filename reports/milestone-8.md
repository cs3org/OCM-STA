# Milestone report: Application Federation (M8)

This report covers the work carried out for Milestone M8 of the OCM-STA
project. The milestone focuses on implementing applications federation in
Nextcloud, following the CERNBox model and targeting in particular JupyterHub,
Nextcloud Talk, and Office-like web apps. It also covers validation of
Nextcloud to CERNBox application sharing and vice versa.

Milestone M8 is tracked in [cs3org/OCM-STA#19][sta-19] and
[cs3org/OCM-STA#20][sta-20]. The public record spans Nextcloud core work for
HTTP Message Signatures, notifications, and OCM discovery; the paired
JupyterHub sender integration and remote-webapp receiver application; and the
CERNBox and Reva discovery and WebApp-protocol work. It also includes the
milestone container images, generated OCM Test Suite coverage, and published
observatory records for the WebApp Share scenarios.

The contractual milestone deadline in the upstream milestone list is
2026-06-30.

## Executive summary

Milestone M8 combines the Nextcloud, CERNBox, and test-suite work needed for
OCM WebApp sharing. Nextcloud gained RFC 9421 HTTP Message Signatures and OCM
discovery support. Its compatibility changes let applications react to
incoming OCM notifications and let federation providers contribute different
protocols for the same resource type without duplicate discovery entries.

The JupyterHub integration provides the sender-side WebApp share path and its
OCM Integration Protocol back channel. The paired `ocmremotewebapp`
application provides the Nextcloud receiver-side share, token-exchange, and
presentation path. The Reva work adds the associated OCM discovery and
WebApp-protocol handling for the CERNBox layer.

The OCM Test Suite now defines generated `webapp-share` coverage for
Nextcloud v35 -> Nextcloud v35 and Nextcloud v35 -> CERNBox v11. In the
linked CI run, both WebApp Share test jobs completed successfully. The public
record also includes published observatory records and recordings for both
cells.

## Contractual milestone definition

According to the project milestone list, Milestone M8 is defined as:

- Milestone M8: Application Federation
  - Implement applications federation in Nextcloud, following the CERNBox
    model and targeting in particular JupyterHub, Nextcloud Talk, and
    Office-like web apps.
  - Validate Nextcloud to CERNBox application sharing and vice versa.

Deadline: 2026-06-30

## Work performed

### 1. Milestone tracker and coordination

- **[cs3org/OCM-STA#19][sta-19]** - Implement applications federation in
  Nextcloud.

- **[cs3org/OCM-STA#20][sta-20]** - Validate Nextcloud to CERNBox application
  sharing and vice versa.
  - Its evidence comment links the implementation pull requests, OCM Test
    Suite run, published observatory records, and recordings used below.

### 2. Nextcloud core and OCM protocol support

- **[nextcloud/server#60136][nc-60136]** (merged)
  - Adds dual-stack HTTP Message Signatures support, including RFC 9421
    signing and verification primitives, OCM key management, JWKS
    publication, and OCM HTTP-signature capability advertisement.

- **[nextcloud/server#61201][nc-61201]** (merged)
  - Adds the public `OCMNotificationReceivedEvent` so applications can react
    to incoming OCM notifications.

- **[nextcloud/server#61126][nc-61126]** (merged)
  - Merges protocols for matching OCM resource-type names in discovery,
    allowing a resource type to advertise its combined protocol support.

- **[nextcloud/documentation#15047][nc-docs-15047]** (merged)
  - Documents the OCM signing-key lifecycle commands introduced with the HTTP
    Message Signatures work.

### 3. WebApp applications and JupyterHub integration

- **[SUNET/ocmremotewebapp][ocmremotewebapp]**
  - Receives and presents OCM WebApp shares in Nextcloud.
  - Advertises `webapp-receive` support for the `folder` resource type with
    `blank` and `iframe` targets.
  - Handles the WebApp share payload, token exchange, incoming-share
    lifecycle, and receiver presentation surface.

- **[SUNET/nextcloud-integration_jupyterhub#23][jupyterhub-23]** (merged)
  - Registers WebDAV alongside WebApp support for the sender-side JupyterHub
    share resource.
  - Is linked from the M8 validation tracker and records the WebDAV resource
    change used by the current WebApp share flow.

- **[SUNET/nextcloud-integration_jupyterhub][jupyterhub]**
  - Provides the Nextcloud sender-side integration, WebApp share dialog,
    remote capability discovery, and JupyterHub OCM Integration Protocol back
    channel.

### 4. CERNBox and Reva compatibility work

- **[cs3org/reva#5611][reva-5611]** (merged)
  - Reworks OCM discovery and consumes remote discovery metadata for WebApp
    support.

- **[cs3org/reva#5664][reva-5664]** (merged)
  - Introduces the OCM WebApp protocol, including share permissions,
    presentation targets, WebApp payload handling, and protocol tests.

- **[cs3org/reva#5695][reva-5695]** (open)
  - Normalizes remote OCM user identifiers used while creating shares.

- **[cs3org/reva#5696][reva-5696]** (open)
  - Resolves single-file OCM shares over WebDAV.

- **[cs3org/reva#5698][reva-5698]** (open)
  - Normalizes user identifiers on incoming OCM shares.

- **[cs3org/reva#5701][reva-5701]** (open) and
  **[cernbox/web#260][cernbox-web-260]** (open)
  - Record the ScienceMesh `OpenInApp` launch support and the CERNBox WebApp
    user-interface work linked from the M8 tracker.

### 5. OCM Test Suite, containers, and published evidence

- **[MahdiBaghbani/containers#20][containers-20]** (merged)
  - Adds the Milestone M8 WebApp share image line and supporting services used
    by the test environment.

- **[cs3org/ocm-test-suite#227][ocmts-227]** (merged)
  - Adds the generated `webapp-share` flow, Cypress scenarios, CI workflow
    assets, and published flow metadata.

- **[MahdiBaghbani/ocm-web-site#7][site-7]** (merged)
  - Adds the WebApp Share flow to the observatory presentation and data
    contract.

- The generated WebApp Share matrix defines:
  - `webapp-share__nextcloud-v35__nextcloud-v35`
  - `webapp-share__nextcloud-v35__cernbox-v11`

- **[GitHub Actions run 29088983177][run-29088983177]**
  - Includes the generated WebApp Share matrix and its aggregate and
    publishing stages.
  - **[Nextcloud v35 -> CERNBox v11][run-nc-cernbox]** completed
    successfully.
  - **[Nextcloud v35 -> Nextcloud v35][run-nc-nc]** completed successfully.

- Published observatory records:
  - **[Nextcloud v35 -> Nextcloud v35][obs-nc-nc]**
  - **[Nextcloud v35 -> CERNBox v11][obs-nc-cernbox]**

- Published recordings:
  - **[Nextcloud -> Nextcloud][video-nc-nc]**
  - **[Nextcloud -> CERNBox][video-nc-cernbox]**

## Specification changes

The M8 implementation work used the evolving OCM WebApp specification and
OCM Integration Protocol. These specification pull requests are included as
technical context; their formal reporting remains with M10.

- **[cs3org/OCM-API#356][ocm-api-356]** (merged)
  - Adds the WebApp-sharing design document.

- **[cs3org/OCM-API#367][ocm-api-367]** (closed)
  - Contains an earlier WebApp protocol proposal whose work was incorporated
    into the successor change.

- **[cs3org/OCM-API#368][ocm-api-368]** (merged)
  - Updates WebApp discovery, permissions, targets, shared-secret handling,
    and the WebApp share schema.

- **[cs3org/OCM-API#370][ocm-api-370]** (merged)
  - Adds the Open Cloud Mesh Integration Protocol draft used for
    protocol-server integration, including web application platforms.

## Results vs milestone goals

### Goal 1: Implement applications federation in Nextcloud

The implementation record for this goal includes the merged Nextcloud core
changes, the `ocmremotewebapp` receiver application, and the
`nextcloud-integration_jupyterhub` sender integration. The latter provides
the JupyterHub WebApp sharing path, remote capability discovery, and its OCM
Integration Protocol back channel.

The WebApp protocol and discovery work is recorded in
**[cs3org/reva#5611][reva-5611]**, **[cs3org/reva#5664][reva-5664]**,
and the referenced OCM-API specification work.

### Goal 2: Validate Nextcloud to CERNBox application sharing and vice versa

The public validation record for this goal is centered on
**[cs3org/ocm-test-suite#227][ocmts-227]**, the generated WebApp Share
matrix, and **[GitHub Actions run 29088983177][run-29088983177]**.

In that CI run, both generated WebApp Share test jobs completed successfully:

- **[Nextcloud v35 -> CERNBox v11][run-nc-cernbox]**
- **[Nextcloud v35 -> Nextcloud v35][run-nc-nc]**

Each WebApp Share scenario creates the contact relationship, creates and
submits the JupyterHub WebApp share from the sender, accepts the incoming
share on the receiver, launches the remote WebApp, and records the JupyterLab
user-interface proof together with screenshots, video, logs, and MITM
material.

The published observatory records and recordings provide the corresponding
review surfaces:

- **[Nextcloud v35 -> Nextcloud v35 observatory record][obs-nc-nc]**
  and **[recording][video-nc-nc]**
- **[Nextcloud v35 -> CERNBox v11 observatory record][obs-nc-cernbox]**
  and **[recording][video-nc-cernbox]**

[sta-19]: https://github.com/cs3org/OCM-STA/issues/19
[sta-20]: https://github.com/cs3org/OCM-STA/issues/20
[nc-60136]: https://github.com/nextcloud/server/pull/60136
[nc-61201]: https://github.com/nextcloud/server/pull/61201
[nc-61126]: https://github.com/nextcloud/server/pull/61126
[nc-docs-15047]: https://github.com/nextcloud/documentation/pull/15047
[ocmremotewebapp]: https://github.com/SUNET/ocmremotewebapp
[jupyterhub]: https://github.com/SUNET/nextcloud-integration_jupyterhub
[jupyterhub-23]: https://github.com/SUNET/nextcloud-integration_jupyterhub/pull/23
[reva-5611]: https://github.com/cs3org/reva/pull/5611
[reva-5664]: https://github.com/cs3org/reva/pull/5664
[reva-5695]: https://github.com/cs3org/reva/pull/5695
[reva-5696]: https://github.com/cs3org/reva/pull/5696
[reva-5698]: https://github.com/cs3org/reva/pull/5698
[reva-5701]: https://github.com/cs3org/reva/pull/5701
[cernbox-web-260]: https://github.com/cernbox/web/pull/260
[containers-20]: https://github.com/MahdiBaghbani/containers/pull/20
[ocmts-227]: https://github.com/cs3org/ocm-test-suite/pull/227
[site-7]: https://github.com/MahdiBaghbani/ocm-web-site/pull/7
[run-29088983177]: https://github.com/cs3org/ocm-test-suite/actions/runs/29088983177
[run-nc-cernbox]: https://github.com/cs3org/ocm-test-suite/actions/runs/29088983177/job/86353137256
[run-nc-nc]: https://github.com/cs3org/ocm-test-suite/actions/runs/29088983177/job/86353137263
[obs-nc-nc]: https://cs3org.github.io/ocm-test-suite/observatory/?cell=webapp-share__nextcloud-v35__nextcloud-v35&run=20260710t113934-4e4db143
[obs-nc-cernbox]: https://cs3org.github.io/ocm-test-suite/observatory/?cell=webapp-share__nextcloud-v35__cernbox-v11&run=20260710t113934-696bbe0f
[video-nc-nc]: https://github.com/user-attachments/assets/be79d049-8d78-42fe-8663-bc3eedfed567
[video-nc-cernbox]: https://github.com/user-attachments/assets/a04cf8b9-e78e-4f91-aa67-33895be14393
[ocm-api-356]: https://github.com/cs3org/OCM-API/pull/356
[ocm-api-367]: https://github.com/cs3org/OCM-API/pull/367
[ocm-api-368]: https://github.com/cs3org/OCM-API/pull/368
[ocm-api-370]: https://github.com/cs3org/OCM-API/pull/370
