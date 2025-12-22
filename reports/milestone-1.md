# Milestone M1 Report - Directory Service and Where-Are-You-From UI in CERNBox

This report covers work completed for **Milestone M1 - Directory Service and Where-Are-You-From UI in CERNBox** under the Open Cloud Mesh (OCM) STA funded activities. The reporting period spans from November 1, 2025 to December 19, 2025, with a milestone deadline of December 31, 2025.

**Scope notes**: Open pull requests are included as work performed (clearly marked as open). Aligned upstream and alignment work pull requests are included as voluntary work that supports the milestone and the wider OCM ecosystem, and are not billed unless explicitly stated otherwise.

## Executive summary

During the reporting period, we implemented the core building blocks for a Directory Service backed **Where-Are-You-From (WAYF)** flow and delivered conformance tests that exercise **CERNBox <-> Nextcloud** sharing when Directory Service lookup and the WAYF UI are involved.

Key outcomes include:

- Backend capabilities for WAYF discovery/federation management in Reva.
- WAYF UI and invitation workflow improvements for CERNBox (submitted; pending merge in the relevant downstream repo at the time of writing).
- New OCM Test Suite coverage for WAYF flows (merged), plus follow-up fixes to stabilize the new tests.
- Supporting containerized test infrastructure work in DockyPody (OCM containers) (supporting context; not billed), enabling reproducible multi-service environments and TLS handling.

## Contractual milestone definition

- Implement WAYF UI with integration of one or more Directory Services, following the current Nextcloud developments
- Conformance tests: extend Test Suite to validate that sharing between CERNBox and Nextcloud works also when a Directory Service lookup and a Where-are-you-from UI page is involved.

## Work performed

### Tracker items (milestone tracking)

- **[cs3org/OCM-STA#1](https://github.com/cs3org/OCM-STA/issues/1)** (closed): "Implement WAYF UI with integration of one or more Directory Services, following the current Nextcloud developments"
- **[cs3org/OCM-STA#2](https://github.com/cs3org/OCM-STA/issues/2)** (closed): "Conformance tests: extend Test Suite ..."

### Directory Service / WAYF discovery backend (Reva)

- **[cs3org/reva#5385](https://github.com/cs3org/reva/pull/5385)** (Merged): Added WAYF-specific discovery and federation endpoints to the ScienceMesh package to support dynamic OCM provider discovery and federation management.
- **[cs3org/reva#5389](https://github.com/cs3org/reva/pull/5389)** (Draft, volunteer hardening work): Strengthens OCM client security (for example SSRF, DNS rebinding, and resource exhaustion mitigations) in the context of remote discovery (depends on #5385) and is tracked as follow-up work aligned with this milestone rather than part of the funded deliverable.

### WAYF UI / invitation workflow (CERNBox and Nextcloud-adjacent work)

- **[cernbox/web#228](https://github.com/cernbox/web/pull/228)** (Open): Implements the WAYF page and enhances the invitation workflow to let users discover/select their home provider when accepting federated invitations (noted as depending on `cs3org/reva#5385`).
- **[sara-nl/nextcloud-contacts#14](https://github.com/sara-nl/nextcloud-contacts/pull/14)** (Open): Improves OCM invite UX (invite email optional, improved copy options including WAYF link formats) and includes a fix for CSRF when accepting an invite redirected from WAYF.

### Conformance tests (OCM Test Suite)

- **[cs3org/ocm-test-suite#208](https://github.com/cs3org/ocm-test-suite/pull/208)** (Merged): Adds end-to-end WAYF tests for CERNBox and Nextcloud. The PR notes supporting infrastructure work in DockyPody to enable running these tests reliably in a containerized environment.
- **[cs3org/ocm-test-suite#209](https://github.com/cs3org/ocm-test-suite/pull/209)** (Merged): Fixes cleanup logic to prevent spurious WAYF-related test failures.
- **[cs3org/ocm-test-suite#211](https://github.com/cs3org/ocm-test-suite/pull/211)** (Merged): Fixes CERNBox->CERNBox user searching issue in WAYF tests.
- **[Live tests preview on the website](https://cs3org.github.io/ocm-test-suite)** (via GitHub Pages): At the bottom of the page, in the "WAYF Directory Service Tests" section, you can see the matrix of conformance tests that exercise the WAYF flow.

### Supporting test infrastructure (DockyPody / Open Cloud Mesh Containers)

Note: This infrastructure work is included as supporting context and is not billed under this milestone.

To support reproducible integration testing (including WAYF flows), we used and extended DockyPody: a Nushell-based build system and repository of container build scripts/resources for OCM services.

- **Repository**: **[MahdiBaghbani/containers](https://github.com/MahdiBaghbani/containers)**
- **What it provides (high level)**:
  - Build orchestration for OCM service images (multi-platform/versioned builds, dependency resolution).
  - TLS certificate generation and build-time TLS handling designed for multi-service test environments.
  - A "selective TLS certificate copying" approach to keep images minimal and reduce certificate exposure by copying only the certificates required by each service.

This infrastructure work supports the conformance testing deliverable (and is referenced in the test-suite PR narrative), enabling the multi-container environment needed to validate CERNBox/Nextcloud WAYF flows.

### Additional aligned volunteer work (not billed; included for transparency and ecosystem benefit)

The following PRs are linked from the tracker issue but are explicitly described (in their PR summaries) as **not directly part of the STA funded work**. They are included here as aligned upstreaming/alignment work done voluntarily to benefit the broader OCM ecosystem and keep implementations consistent.

Representative examples:

- **[owncloud/reva#432](https://github.com/owncloud/reva/pull/432)** (Open): WAYF-specific discovery/federations endpoints (aligned with the Reva work above).
- **[opencloud-eu/reva#393](https://github.com/opencloud-eu/reva/pull/393)** (Merged): Port/alignment of WAYF-specific discovery/federations endpoints.
- **[opencloud-eu/opencloud#1714](https://github.com/opencloud-eu/opencloud/pull/1714)** (Merged): WAYF configuration support (aligned port; depends on opencloud-eu/reva#393).

Full list is provided in Appendix A.

## Results vs milestone goals

### Goal 1 - Implement WAYF UI with Directory Service integration

**Work completed (with status)**:

- Backend WAYF discovery/federation endpoints: **[cs3org/reva#5385](https://github.com/cs3org/reva/pull/5385)** (Merged).
- CERNBox WAYF UI + invitation flow: **[cernbox/web#228](https://github.com/cernbox/web/pull/228)** (Open; submitted for downstream review/merge at the time of writing).
- Nextcloud-adjacent improvements to invite UX (including WAYF link formats and a CSRF fix): **[sara-nl/nextcloud-contacts#14](https://github.com/sara-nl/nextcloud-contacts/pull/14)** (Open; submitted for upstream review/merge at the time of writing).

**How this relates to the milestone definition**:

- The work above implements the WAYF backend and UI described in the milestone text and is exercised by automated conformance tests.
- Some user-facing integration pull requests in downstream projects remain open at the time of writing; these are deployable integration steps subject to local review and follow-up fixes in downstream releases.

### Goal 2 - Conformance tests for CERNBox <-> Nextcloud sharing with Directory Service + WAYF UI

**Delivered work**:

- New WAYF conformance tests merged in **[cs3org/ocm-test-suite#208](https://github.com/cs3org/ocm-test-suite/pull/208)**.
- Stabilization/fixes merged in **[cs3org/ocm-test-suite#209](https://github.com/cs3org/ocm-test-suite/pull/209)** and **[cs3org/ocm-test-suite#211](https://github.com/cs3org/ocm-test-suite/pull/211)**.
- Supporting container test infrastructure (supporting context; not billed) is documented and maintained in **[MahdiBaghbani/containers](https://github.com/MahdiBaghbani/containers)**, referenced by the test-suite PR narrative.

**How this relates to the milestone definition**:

- The conformance tests described above are merged and runnable, and the supporting infrastructure is available to execute them in a reproducible multi-service environment.

## Notes on remaining work

Several integration pull requests remain open at the time of writing (notably **[cernbox/web#228](https://github.com/cernbox/web/pull/228)** and **[sara-nl/nextcloud-contacts#14](https://github.com/sara-nl/nextcloud-contacts/pull/14)**), which represent deployable integration points for the WAYF flow and are expected to be reviewed and merged through the normal downstream release processes. In addition, hardening work such as **[cs3org/reva#5389](https://github.com/cs3org/reva/pull/5389)** is tracked as volunteer follow-up security work aligned with this milestone, and is not required to make use of the delivered WAYF endpoints and conformance tests.

## Appendix A - Additional aligned volunteer PRs (not billed)

- **[owncloud/reva#432](https://github.com/owncloud/reva/pull/432)** (Open): WAYF-specific discovery/federations endpoints (aligned with the Reva work above).
- **[opencloud-eu/reva#393](https://github.com/opencloud-eu/reva/pull/393)** (Merged): Port/alignment of WAYF-specific discovery/federations endpoints.
- **[owncloud/ocis#11758](https://github.com/owncloud/ocis/pull/11758)** (Open): WAYF configuration wiring for Reva OCM service (aligned port; includes follow-up fix to read directory service from URL).
- **[opencloud-eu/opencloud#1714](https://github.com/opencloud-eu/opencloud/pull/1714)** (Merged): WAYF configuration support (aligned port; depends on opencloud-eu/reva#393).
- **[owncloud/web#13243](https://github.com/owncloud/web/pull/13243)** (Open): WAYF page + invitation workflow (aligned port).
- **[opencloud-eu/web#1486](https://github.com/opencloud-eu/web/pull/1486)** (Open): WAYF page + invitation workflow (aligned port; depends on opencloud-eu/reva#393 and opencloud-eu/opencloud#1714).
