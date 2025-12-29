# Milestone M2 Report - Plugin for running the OCM Test Suite in Nextcloud's and Reva's CI pipelines

This report describes the work carried out towards Milestone M2 of the OCM-STA project. The focus of this milestone is to enable vendors to validate their OCM compliance by running the OCM Test Suite in their CI pipelines, and to provide supporting documentation and test coverage to make those workflows repeatable. For CERNBox, the backend component is Reva, so the CI integration work is implemented in the Reva repository while still describing the CERNBox application as the vendor under test.

The work described here was carried out during 2025 in the lead-up to the milestone deadline of 31 December 2025. It spans upstream vendor pull requests, OCM Test Suite CI support, and a small set of protocol-related changes in OCM-stub to support the test scenarios.

## Executive summary

Milestone M2 has three main goals: submit upstream pull requests to Nextcloud and CERNBox so they can validate OCM compliance in CI, implement a parameterised vendor test matrix using a reference OCM stub, and provide template documentation for each vendor. During the reporting period, upstream pull requests were submitted to add OCM Test Suite matrix workflows for Nextcloud and for CERNBox (via its Reva backend). Supporting changes were merged into the OCM Test Suite to add vendor-specific CI helpers and documentation, and into OCM-stub to implement invite token generation and acceptance flow needed by the invite-link scenarios.

Demo GitHub Actions runs are available for both vendor workflows and show the matrix structure executing and producing artifacts (for example Cypress videos). The workflows are configured as non-blocking, so failing matrix entries are still reported while keeping the overall CI run visible and reviewable.

Overall, the milestone deliverables are implemented and submitted for upstream review, with the vendor CI pull requests currently open in the Nextcloud and Reva repositories.

## Contractual milestone definition

According to the project milestone list, Milestone M2 is defined as:

- Milestone M2: Plugin for running the OCM Test Suite in Nextcloud's and Reva's CI pipelines
  - Produce and submit a PR upstream to Nextcloud and CERNBox, such that they can validate their compliance to the OCM spec
  - Parameterised matrix test for each Vendor V (V to/from Stub, V to/from V, Stub to/from Stub), where the "Stub" is an OCM stub implementation provided by [cs3org/ocm-stub](https://github.com/cs3org/ocm-stub) that is to be considered the reference implementation of the protocol.
  - Template documentation for each Vendor

The official deadline for this milestone is 31 December 2025.

## Work performed

### 1. Upstream vendor CI workflows (Nextcloud and CERNBox)

#### OCM-STA tracker issues

- **[cs3org/OCM-STA#3](https://github.com/cs3org/OCM-STA/issues/3)** (closed) - Produce and submit a PR upstream to Nextcloud and CERNBox
  - Tracks the upstream submission work and links directly to the vendor PRs below.

- **[cs3org/OCM-STA#4](https://github.com/cs3org/OCM-STA/issues/4)** (closed) - Parameterised matrix test for each Vendor
  - Tracks the definition of the matrix test approach and links to the vendor PRs, supporting repositories, and demo runs.

#### Nextcloud upstream PR

- **[nextcloud/server#57274](https://github.com/nextcloud/server/pull/57274)** (open)
  - Adds a dedicated GitHub Actions workflow that runs the OCM Test Suite against the Nextcloud server using a small, fixed share-with matrix.
  - Includes a summary job that aggregates per-combination results into a compact table in the Actions summary.
  - Configured to be non-blocking (continue-on-error), so regressions are visible without failing the entire CI run.

Demo run (demonstration run on a fork repository used to validate the workflow behavior prior to upstream merge):

- **[GitHub Actions run 20552606133](https://github.com/MahdiBaghbani/server/actions/runs/20552606133)**

#### CERNBox (Reva backend) upstream PR

- **[cs3org/reva#5458](https://github.com/cs3org/reva/pull/5458)** (open)
  - Adds a dedicated GitHub Actions workflow that runs the OCM Test Suite against CERNBox using Reva as the backend, using a small, fixed invite-link matrix.
  - Builds a revad binary from the current branch and passes it into the test environment for the matrix entries that use the current CERNBox/Reva code.
  - Configured to be non-blocking (continue-on-error), so the complete matrix is exercised and results are captured even when some combinations fail.

Demo run (demonstration run on a fork repository used to validate the workflow behavior prior to upstream merge):

- **[GitHub Actions run 20552747067](https://github.com/MahdiBaghbani/reva/actions/runs/20552747067)**

### 2. Supporting work to enable vendor matrix tests

#### OCM Test Suite vendor CI support and documentation

- **[cs3org/ocm-test-suite#214](https://github.com/cs3org/ocm-test-suite/pull/214)** (merged)
  - Adds vendor-specific CI helpers and workflows for Nextcloud and CERNBox/Reva.
  - Implements the OCM Test Suite CI orchestration used by the vendor workflows: run a small parameterised matrix via the `dev-stock` image, capture per-combination status, and publish a compact summary table while keeping Cypress artifacts for review.
  - Adds detailed documentation for the vendor CI workflows (referenced from the M2 tracker issues as the documentation deliverable).
  - Provides the shared basis used by the upstream vendor PRs to run the OCM Test Suite in a reproducible way.

#### OCM-stub changes for invite-token scenarios

- **[cs3org/OCM-stub#14](https://github.com/cs3org/OCM-stub/pull/14)** (merged)
  - Implements invite token generation and invite acceptance handling to support invite-link test scenarios.
  - Introduces an ocm-core module for protocol logic and refines invite acceptance behavior and response structure.

### 3. Vendor documentation template

The vendor CI documentation deliverable is tracked as **[cs3org/OCM-STA#5](https://github.com/cs3org/OCM-STA/issues/5)** (closed) and is implemented in **[cs3org/ocm-test-suite#214](https://github.com/cs3org/ocm-test-suite/pull/214)** (merged), which is referenced from the tracker issue as the place where vendor documentation for the current state of the CI plugin was generated.

## Results vs milestone goals

### Goal 1: Produce and submit a PR upstream to Nextcloud and CERNBox

The tracker issue **[cs3org/OCM-STA#3](https://github.com/cs3org/OCM-STA/issues/3)** links to two upstream pull requests: **[nextcloud/server#57274](https://github.com/nextcloud/server/pull/57274)** for Nextcloud and **[cs3org/reva#5458](https://github.com/cs3org/reva/pull/5458)** for CERNBox (via its Reva backend). Both pull requests add CI workflows intended to run the OCM Test Suite as a vendor-facing regression check and are available for upstream review and validation.

### Goal 2: Parameterised matrix test for each Vendor and the reference Stub

The tracker issue **[cs3org/OCM-STA#4](https://github.com/cs3org/OCM-STA/issues/4)** ties together the concrete implementations of the matrix approach across Nextcloud and CERNBox/Reva. The upstream vendor PRs implement fixed matrices for the relevant scenarios and provide summary reporting and artifacts. Supporting work in **[cs3org/ocm-test-suite#214](https://github.com/cs3org/ocm-test-suite/pull/214)** provides shared CI and documentation infrastructure for these workflows, while **[cs3org/OCM-stub#14](https://github.com/cs3org/OCM-stub/pull/14)** implements invite-token handling needed for the invite-link scenarios.

The two demo runs (**[GitHub Actions run 20552606133](https://github.com/MahdiBaghbani/server/actions/runs/20552606133)** and **[GitHub Actions run 20552747067](https://github.com/MahdiBaghbani/reva/actions/runs/20552747067)**) show the matrix jobs executing and producing artifacts. Some matrix entries report failures (for example vendor-to-stub directions), but the workflows are configured to keep the runs reviewable and to surface results for all combinations.

In this milestone, "V to/from V" is implemented as same-vendor loopback testing (Nextcloud <-> Nextcloud for the Nextcloud workflow, and CERNBox <-> CERNBox for the CERNBox/Reva workflow). The Nextcloud workflow exercises the `share-with` scenario, while the CERNBox/Reva workflow exercises the `invite-link` scenario. Each workflow uses a four-entry matrix that includes Stub <-> Stub, Stub <-> Vendor, Vendor <-> Stub, and Vendor <-> Vendor, with the concrete job structure and parameters described in the vendor CI documentation linked in Goal 3.

### Goal 3: Template documentation for each Vendor

The tracker issue **[cs3org/OCM-STA#5](https://github.com/cs3org/OCM-STA/issues/5)** and the merged pull request **[cs3org/ocm-test-suite#214](https://github.com/cs3org/ocm-test-suite/pull/214)** provide the documentation deliverable. The documentation is designed to help vendors run and interpret the OCM Test Suite CI workflows and to tie the workflow behavior back to the milestone goals.

Permanent documentation links:

- **Reva / CERNBox vendor CI**: **[cs3org/ocm-test-suite docs/vendor-ci/vendor-ci-reva.md](https://github.com/cs3org/ocm-test-suite/blob/fabfce69fff106485331b1e288313d01fc304131/docs/vendor-ci/vendor-ci-reva.md)**
- **Nextcloud vendor CI**: **[cs3org/ocm-test-suite docs/vendor-ci/vendor-ci-nextcloud.md](https://github.com/cs3org/ocm-test-suite/blob/fabfce69fff106485331b1e288313d01fc304131/docs/vendor-ci/vendor-ci-nextcloud.md)**
