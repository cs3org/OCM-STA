## Milestone report: Incorporate CERNBox share-flow test in the OCM Test Suite (M4)

This report describes the work carried out towards Milestone M4 of the OCM-STA project. The focus of this milestone is to add a full CERNBox share-flow test to the OCM Test Suite and to implement the fixes needed so that the sharing phase passes when run against CERNBox and its peers. The work described here was carried out during 2025 in the lead-up to the milestone deadline of 31 December 2025 and spans changes across the OCM Test Suite and several related services (Reva and its forks, oCIS, and OpenCloud).

The scope of this report is limited to the activities directly related to Milestone M4 as defined in the project milestone list. Follow-up work for other platforms, such as Nextcloud code-flow and token handling, is referenced where relevant but belongs to other milestones.

## Executive summary

Milestone M4 (Incorporate CERNBox share-flow test in the OCM Test Suite) has two main goals: first, to add complete CERNBox share-flow coverage to the OCM Test Suite; and second, to implement the fixes required so that the sharing phase passes when executed against CERNBox and its peers. During the reporting period, a dedicated pull request in the OCM Test Suite added end-to-end CERNBox share-flow tests, while a series of changes in Reva and its downstream forks addressed OCM address parsing, WebDAV protocol semantics, invite expiry handling, and other issues needed for these tests to succeed.

As a result, the CERNBox share-flow tests are now integrated into the OCM Test Suite and are exercised in recent test runs. The current pass/fail status depends on the provider combination; a recent snapshot is maintained in the [OCM Test Suite compatibility matrix](https://github.com/cs3org/ocm-test-suite/blob/main/docs/4.2-compatibility-matrix.md).

A remaining limitation, that Nextcloud cannot yet receive OCM 1.2 shares in some scenarios, is explicitly tracked as follow-up work in separate Milestone M3 issues rather than as part of M4.

## Contractual milestone definition

According to the project milestone list, Milestone M4 is defined as:

- Milestone M4: Incorporate CERNBox share-flow test in the OCM Test Suite
  - Ensure full coverage for CERNBox in the OCM Test Suite (the assumption is that some tests do not pass).
  - Implement any necessary fix for the sharing phase to pass the OCM Test Suite.

The official deadline for this milestone is 31 December 2025.

## Work performed

### 1. CERNBox share-flow coverage in the OCM Test Suite

#### OCM-STA tracker issues

- **[cs3org/OCM-STA#8](https://github.com/cs3org/OCM-STA/issues/8)** (closed) - Ensure full coverage for CERNBox in the OCM Test Suite  
  - Tracks the definition and completion of Milestone M4, including linking it to the Sovereign Tech funded activities project and to the M4 entry in the milestone list.  
  - Acts as the umbrella issue for implementing CERNBox coverage in the OCM Test Suite.  
  - References the implementation work in the OCM Test Suite repository once it is available.

#### OCM Test Suite implementation

- **[cs3org/ocm-test-suite#210](https://github.com/cs3org/ocm-test-suite/pull/210)** (merged)  
  - Adds a dedicated CERNBox share-flow scenario to the OCM Test Suite, making CERNBox v2 a first class platform in the test matrix.  
  - Refactors existing scripts and configuration to target CERNBox v2 and introduces a full share-flow test that exercises invite creation, acceptance, and remote access from CERNBox.  
  - Improves test infrastructure reliability through additional commits, including:
    - Updating invite-link scripts and provider configurations to match the current OCM deployments.
    - Addressing TLS and certificate handling issues and enhancing Docker network cleanup to avoid interference between runs.
  - Explicitly references the M4 tracking issue in OCM-STA, tying the test-suite work back to the contractual milestone.

Taken together, these artefacts show that CERNBox is now explicitly covered by the OCM Test Suite with a full share-flow test, rather than being exercised only indirectly or through partial scenarios.

For a snapshot of recent test outcomes across provider combinations, see the OCM Test Suite compatibility matrix.

### 2. Core Reva changes required for passing share flows

Several changes in Reva were necessary to ensure that OCM share flows involving CERNBox behave correctly and consistently.

#### OCM address parsing and invite handling (cs3org/reva)

- **[cs3org/reva#5375](https://github.com/cs3org/reva/pull/5375)** - Fix parsing of OCM address with multiple at-signs  
  - Introduces a fix for parsing OCM addresses that contain more than one at-sign, preventing mis-interpretation of remote identifiers in such cases.  
  - Improves robustness of OCM invite processing where complex addresses are involved.

- **[cs3org/reva#5384](https://github.com/cs3org/reva/pull/5384)** (superseded, closed) - Fix parsing of OCM address with multiple at-signs  
  - Provides a follow-up refinement of the same parsing problem, later closed in favour of a more comprehensive solution.  
  - References the underlying issue and clarifies that the final fix is consolidated elsewhere.

- **[cs3org/reva#5415](https://github.com/cs3org/reva/pull/5415)** - Bugfix: set correct user type when accepting OCM invites  
  - Adjusts how user types are assigned when accepting OCM invites so that invited users are classified correctly in Reva's internal model.  
  - Prevents mismatches that can cause share acceptance to fail or behave inconsistently across providers.

- **[cs3org/reva#5418](https://github.com/cs3org/reva/pull/5418)** - Bugfix: fix inverted expiry check in JSON invite repository  
  - Corrects an inverted expiry check in the JSON invite repository, ensuring that expired invites are rejected and valid invites are accepted as expected.  
  - Reduces flakiness in test scenarios where invite expiry is relevant.

#### Platform and deployment improvements

- **[cs3org/reva#5404](https://github.com/cs3org/reva/pull/5404)** - Add LocalFS virtual namespace support  
  - Adds support for a LocalFS virtual namespace in Reva.  
  - Provides flexibility in how storage backends are exposed to OCM, which is useful for local and CERNBox-like deployments used by the test suite.

- **[cs3org/reva#5407](https://github.com/cs3org/reva/pull/5407)** (merged) - Add musl based fully static build target  
  - Introduces a musl based fully static build target for Reva.  
  - Simplifies deployment of Reva as a self-contained binary in test environments, reducing dependence on the underlying system and making the OCM Test Suite runs against CERNBox and related deployments more predictable.

These Reva changes address core behaviour required for reliable OCM invite handling and share acceptance, supporting more reliable CERNBox share-flow execution in the OCM Test Suite.

### 3. OCM WebDAV and compliance fixes in downstream Reva forks

Because OCM share flows for CERNBox interact with multiple Reva deployments, including forks, part of the work focused on aligning WebDAV behaviour and OCM semantics across these codebases.

#### OwnCloud Reva

- **[owncloud/reva#429](https://github.com/owncloud/reva/pull/429)** (closed) - Mahdi incoming (OCM WebDAV and invite fixes)  
  - Bundles multiple OCM related fixes, including resolving an OCM WebDAV protocol entity mismatch and improving invite handling and provider settings.  
  - Contains several commits that clean up OCM specific logic, remove redundant conversions, and fix type mismatches that can affect cross-vendor shares.

- **[owncloud/reva#435](https://github.com/owncloud/reva/pull/435)** - OCM specification compliance (ownCloud fork)  
  - Addresses broader OCM specification compliance in the ownCloud Reva fork.  
  - Is referenced from other work as part of a wider OCM compliance effort, helping ensure that ownCloud based deployments participate correctly in OCM share flows.

#### OpenCloud Reva

- **[opencloud-eu/reva#382](https://github.com/opencloud-eu/reva/pull/382)** (merged) - Bugfix: fix the OCM WebDAV protocol entity mismatch  
  - Fixes a mismatch in the OCM WebDAV protocol entity representation, aligning OpenCloud's behaviour with the expectations of the OCM specification.  
  - Is linked to a corresponding issue in the OpenCloud project and to earlier work where the same bug appeared in multiple places.

- **[opencloud-eu/reva#383](https://github.com/opencloud-eu/reva/pull/383)** (closed, superseded) - [WiP] fix(ocm): OCM specification compliance  
  - Early work in progress pull request that started addressing OCM specification compliance.  
  - Later closed and superseded by a more complete effort.

- **[opencloud-eu/reva#434](https://github.com/opencloud-eu/reva/pull/434)** - fix(ocm): OCM specification compliance  
  - Consolidates OCM specification compliance changes in the OpenCloud Reva fork.  
  - Builds on and supersedes the work from pull request 383, aiming to ensure OCM behaviour is consistent and interoperable with other providers.

These downstream fixes ensure that the Reva deployments used by CERNBox and its peers implement consistent OCM semantics, reducing interoperability issues encountered when running CERNBox share-flow scenarios.

### 4. oCIS behaviour and external dependencies

- **[owncloud/ocis#11735](https://github.com/owncloud/ocis/issues/11735)** - oCIS cannot receive OCM shares from other EFSS  
  - Captures an interoperability issue observed during cross-provider testing, including OCM address formatting and identifier handling expectations across implementations.  
  - Provides context for some of the non-CERNBox-specific fixes referenced from the M4 tracker issue, and documents how this limitation was resolved in upstream work (the issue is now closed).  
  - The current cross-provider status is summarized in the OCM Test Suite compatibility matrix.

### 5. Cross-milestone work in OCM-STA related to Nextcloud

Although not part of M4's scope, two Milestone M3 issues are closely related to the residual limitations noted when closing the M4 tracker issue:

- **[cs3org/OCM-STA#29](https://github.com/cs3org/OCM-STA/issues/29)** (M3) - Implement bearer auth for accessing shares, both incoming and outgoing  
  - Tracks work to add bearer authentication for accessing OCM shares in both directions.  
  - Provides the basis for robust handling of share access in Nextcloud and other providers.

- **[cs3org/OCM-STA#30](https://github.com/cs3org/OCM-STA/issues/30)** (M3, open) - Implement refresh of tokens and code capability  
  - Covers the implementation of token refresh and OCM code-flow capability in Nextcloud.  
  - Is explicitly referenced from the M4 tracker issue comment that notes Nextcloud's current inability to receive OCM 1.2 shares; this limitation is therefore treated as follow-up work under Milestone M3.

These issues are mentioned here for completeness, but they belong to a different milestone and act as documented follow-ups to the work described under M4.

## Results vs milestone goals

### Goal 1: Ensure full coverage for CERNBox in the OCM Test Suite

The combination of the OCM-STA tracker issue **[cs3org/OCM-STA#8](https://github.com/cs3org/OCM-STA/issues/8)** and the OCM Test Suite pull request **[cs3org/ocm-test-suite#210](https://github.com/cs3org/ocm-test-suite/pull/210)** demonstrates that CERNBox now has dedicated share-flow coverage in the OCM Test Suite. The test suite contains a specific CERNBox share-flow scenario, with CERNBox v2 configured as a primary platform and the necessary scripts, configuration, and infrastructure adjustments in place.

Supporting changes in Reva and its forks (for example **[cs3org/reva#5404](https://github.com/cs3org/reva/pull/5404)**, **[cs3org/reva#5407](https://github.com/cs3org/reva/pull/5407)**, **[opencloud-eu/reva#382](https://github.com/opencloud-eu/reva/pull/382)**, **[owncloud/reva#429](https://github.com/owncloud/reva/pull/429)**) address interoperability issues encountered during share-flow execution and improve consistency across the services used in the test environment.

In current OCM Test Suite runs, share-flow scenarios pass for some provider combinations but not all. A recent snapshot is maintained in the [OCM Test Suite compatibility matrix](https://github.com/cs3org/ocm-test-suite/blob/main/docs/4.2-compatibility-matrix.md).

### Goal 2: Implement any necessary fix for the sharing phase to pass the OCM Test Suite

The second goal focuses on the fixes required so that the sharing phase itself passes when the OCM Test Suite is run against CERNBox and its counterpart services. The work tracked under **[cs3org/OCM-STA#9](https://github.com/cs3org/OCM-STA/issues/9)** and the linked pull requests covers several categories of fixes:

- OCM address parsing and invite robustness (for example **[cs3org/reva#5375](https://github.com/cs3org/reva/pull/5375)**, **[cs3org/reva#5384](https://github.com/cs3org/reva/pull/5384)**) ensure that complex OCM addresses are interpreted correctly, preventing failures in invite creation and acceptance.  
- Invite expiry and user-type correctness (for example **[cs3org/reva#5415](https://github.com/cs3org/reva/pull/5415)**, **[cs3org/reva#5418](https://github.com/cs3org/reva/pull/5418)**) address subtle bugs in how invites are stored and validated, so that valid invites are accepted and expired ones are rejected as expected.  
- WebDAV protocol alignment and OCM compliance (for example **[opencloud-eu/reva#382](https://github.com/opencloud-eu/reva/pull/382)**, **[opencloud-eu/reva#434](https://github.com/opencloud-eu/reva/pull/434)**, **[owncloud/reva#429](https://github.com/owncloud/reva/pull/429)**, **[owncloud/reva#435](https://github.com/owncloud/reva/pull/435)**) remove protocol mismatches and make sure that WebDAV endpoints behave in line with the OCM specification across the different Reva deployments used in the test environment.  
- Deployment and build improvements (for example **[cs3org/reva#5407](https://github.com/cs3org/reva/pull/5407)**) ensure that Reva can be built and deployed in a way that is reproducible and suitable for the automated test context.

The closing comment in **[cs3org/OCM-STA#9](https://github.com/cs3org/OCM-STA/issues/9)** states that, in the OCM Test Suite, sharing now works for Nextcloud, CERNBox, oCIS, and OpenCloud, with the important caveat that Nextcloud cannot yet receive OCM 1.2 shares. That residual limitation is explicitly tracked in Milestone M3 issues **[cs3org/OCM-STA#29](https://github.com/cs3org/OCM-STA/issues/29)** and **[cs3org/OCM-STA#30](https://github.com/cs3org/OCM-STA/issues/30)**, which focus on bearer authentication and token/code-flow support in Nextcloud.

From a Milestone M4 perspective, this means that the necessary test coverage has been added and a set of fixes has been implemented across the involved components to address observed share-flow failures. Current pass/fail status depends on the provider combination and is tracked in the OCM Test Suite compatibility matrix.

Broader improvements to Nextcloud's OCM 1.2 behaviour are documented as follow-up work in other milestones rather than being part of M4 itself.
