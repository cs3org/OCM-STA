## Milestone report: Code-flow validation between Nextcloud and CERNBox (M6)

This report covers the work carried out for Milestone M6 of the
OCM-STA project. The milestone focuses on the OCM authorization code
flow between Nextcloud and CERNBox, with particular attention to a
CERNBox to Nextcloud share round-trip that relies on token exchange for
remote access. The work touched four main areas: the OCM Test Suite,
milestone-specific container and image wiring, Reva-side code-flow
support, and the Nextcloud-side changes needed for exchanged tokens and
remote access.

The milestone was originally planned against a 28 February 2026
deadline and was later tracked against a 31 March 2026 deadline in
project communication. This report uses that later date. It also
references the public validation artifacts published on 25 March 2026,
including the later refreshed orchestrator rerun that updated the
public matrix and Pages site. Some implementation pull requests are
still open upstream; they are included here because the validated
runtime used milestone-tagged images and fork-backed branches, while
those pull requests remain the public review artifacts for the same
changes.

## Executive summary

Milestone M6 combines implementation work and validation work. On the
implementation side, the OCM Test Suite gained a dedicated code-flow
matrix for Nextcloud v33 and CERNBox v2, the validation stack was wired
to published milestone images, Reva gained the token-exchange path
needed for the flow, and the paired Nextcloud branch added the
exchanged-token and remote-detection changes required for the runtime
path.

On the validation side, the current public record includes an
orchestrator run, a published compatibility matrix, a GitHub Pages
results site, a code-flow artifact bundle, and per-scenario recordings.
In the latest published matrix, the code-flow section lists all four
cells as passing, including both cross-vendor directions between
CERNBox v2 and Nextcloud v33 as well as the same-platform cases for
CERNBox v2 -> CERNBox v2 and Nextcloud v33 -> Nextcloud v33.

## Contractual milestone definition

According to the project milestone list, Milestone M6 is defined as:

- Milestone M6: Code-flow validation between Nextcloud and CERNBox
  - Validate logic against CERNBox.
  - Implement spec conformance tests: CERNBox to Nextcloud share
    round-trip with token exchange on remote access.

The upstream milestone list still records 28 February 2026. For the
work covered by this report, the milestone was later tracked against a
31 March 2026 deadline, and this report uses that later date.

## Work performed

### 1. Milestone tracker and coordination

- **[cs3org/OCM-STA#16][sta-16]** - Implement spec conformance tests:
  CERNBox to Nextcloud share round-trip with token exchange on remote
  access
  - Serves as the milestone tracking entry point for this implementation
    set.
  - Links the public pull requests in the containers repository, the
    OCM Test Suite, Reva, and Nextcloud.
  - Provides the milestone-level reference point for the implementation
    set described below.

### 2. OCM Test Suite implementation and public validation output

- **[cs3org/ocm-test-suite#216][ocmts-216]** (merged)
  - Adds the dedicated code-flow scenario matrix for Nextcloud v33 and
    CERNBox v2.
  - Introduces the Cypress coverage, scenario wrappers, helper
    functions, workflow wiring, and reporting integration needed for the
    new path.
  - Switches the scenario setup to the published milestone image line
    instead of local-only tags.

- **[GitHub Actions run 23564875382][run-111]** (successful public
  orchestrator rerun on commit `1cbbf84`)
  - Publishes the refreshed compatibility matrix and the site artifacts.
  - Produces the `compatibility-matrix`, `github-pages`, and
    `site-artifacts` outputs referenced in this report.

- **[OCM Compatibility Matrix][matrix]** and the
  **[published results site][site]**
  - Provide the public validation surface for the milestone work.
  - Expose the code-flow matrix, downloadable bundles, and
    per-scenario recordings.

- **[Code-flow artifact bundle][codeflow-bundle]**
  - Publishes the four current code-flow scenario directories:
  - `code-flow-crnbx-v2-crnbx-v2`
  - `code-flow-crnbx-v2-nc-v33`
  - `code-flow-nc-v33-crnbx-v2`
  - `code-flow-nc-v33-nc-v33`

- Current published recordings for the code-flow scenarios:
  - **[CERNBox v2 -> CERNBox v2][video-crnbx-crnbx]**
  - **[CERNBox v2 -> Nextcloud v33][video-crnbx-nc]**
  - **[Nextcloud v33 -> CERNBox v2][video-nc-crnbx]**
  - **[Nextcloud v33 -> Nextcloud v33][video-nc-nc]**

The setup behind these artifacts was specific to the code-flow path. In
the **[platform matrix config][platform-matrix-config]**, `code-flow`
is defined as a dedicated 2x2 matrix covering only the four Nextcloud
v33 and CERNBox v2 pairings. On the Nextcloud side, the suite uses the
dedicated **[Nextcloud code-flow wrapper][nextcloud-codeflow-wrapper]**
and the milestone image line
`ghcr.io/mahdibaghbani/containers/nextcloud-contacts:sta-ocm-m6-debian`,
which is also pulled explicitly by the public **[Nextcloud pull
script][nextcloud-pull-script]**. That image line is backed by
**[MahdiBaghbani/server@sta-milestone-6/ocm-code-flow][nextcloud-branch]**
and the paired Contacts branch **[MahdiBaghbani/nextcloud-contacts@
mahdi/fix/ui-optional-email][contacts-branch]**.

On the CERNBox side, the public **[CERNBox pull script][cernbox-pull-script]**
pulls `ghcr.io/mahdibaghbani/containers/cernbox-revad:sta-ocm-m6-development`
together with `ghcr.io/mahdibaghbani/containers/cernbox-web:testing`.
The **[CERNBox code-flow wrapper][cernbox-codeflow-wrapper]** then
switches on `OCM_ENABLE_CODE_FLOW=true`, adds the
`REVAD_AUTHPROVIDER_OCMSHARECODE_*` and
`REVAD_AUTHPROVIDER_OCMEXCHANGEDTOKEN_*` services, and passes
`OCM_CLIENT_INSECURE` into the internal federation calls. The Reva
image line is backed by **[MahdiBaghbani/reva-opencloud@
sta-milestone-6/ocm-code-flow][reva-branch]**.

These materials are the public test outputs associated with the
milestone's code-flow validation work. The milestone runtime behind them
used the milestone-tagged images and fork-backed branches named above,
while the open upstream pull requests below remain the public review
artifacts for the underlying code changes.

### 3. Milestone-specific container and image wiring

- **[MahdiBaghbani/containers#2][containers-2]** (merged)
  - Replaces local-only image references with a milestone-specific image
    line that can be fetched in CI.
  - Gives the validation setup a stable published image source for the
    code-flow work.

This change is part of the milestone because it makes the validation
environment reproducible outside a local development setup and aligns
the public test runs with the intended milestone images.

### 4. Reva code-flow and token-exchange work

- **[cs3org/reva#5552][reva-5552]** (open)
  - Adds the Reva-side authorization code-flow support, including
    discovery advertisement for token exchange and the `/ocm/token`
    exchange route.
  - Separates share-code validation from exchanged-token validation in
    the authentication path.
  - Includes related runtime fixes in areas such as scope handling,
    persistence coverage, storage routing, and token-handler tests that
    were needed for the end-to-end path exercised in milestone
    validation.

This pull request is the main public implementation artifact for the
backend side of the milestone.

### 5. Nextcloud-side interoperability changes

- **[enriquepablo/server#2][server-2]** (open)
  - Carries the paired Nextcloud changes used in the milestone
    validation.
  - Moves exchanged bearer tokens into dedicated access-token fields
    with persisted expiry.
  - Improves refresh handling and tightens remote detection for Reva
    peers during the remote-access path.

- **[nextcloud/server#57234][nextcloud-57234]** (open, upstream
  background)
  - Provides earlier upstream groundwork for adapting remote-share
    behavior to the OCM specification.
  - Serves as context for the more targeted milestone-specific changes
    in the paired Nextcloud branch.

Together, these changes describe the receiver-side behavior needed for
the tested code-flow path.

## Results vs milestone goals

### Goal 1: Validate logic against CERNBox

The public validation record for this goal is centered on
**[cs3org/ocm-test-suite#216][ocmts-216]**, the public
**[GitHub Actions run 23564875382][run-111]**, the published
**[compatibility matrix][matrix]**, and the four published code-flow
recordings.

In the latest published matrix, the code-flow section lists the
following public results:

- **CERNBox v2 -> CERNBox v2** - published as passed
  (**[scenario run][run-crnbx-crnbx]**)
- **CERNBox v2 -> Nextcloud v33** - published as passed
  (**[scenario run][run-crnbx-nc]**)
- **Nextcloud v33 -> CERNBox v2** - published as passed
  (**[scenario run][run-nc-crnbx]**)
- **Nextcloud v33 -> Nextcloud v33** - published as passed
  (**[scenario run][run-nc-nc]**)

For the specific cross-vendor pairing named in the milestone, both
public Nextcloud/CERNBox directions are currently published as passing.
The latest published rerun also updates the CERNBox v2 -> CERNBox v2
cell from the earlier provisional public failure to a published pass.

### Goal 2: Spec conformance tests for remote access

For the contractual CERNBox -> Nextcloud share round-trip with token
exchange on remote access, the most direct public evidence is the
**[CERNBox v2 -> Nextcloud v33 scenario run][run-crnbx-nc]** together
with its **[published recording][video-crnbx-nc]**. The reciprocal
**[Nextcloud v33 -> CERNBox v2 scenario run][run-nc-crnbx]** and its
**[published recording][video-nc-crnbx]** provide the matching
cross-vendor path in the other direction.

The test implementation for this goal is described in
**[cs3org/ocm-test-suite#216][ocmts-216]**. That pull request adds the
code-flow matrix itself together with the Cypress coverage, helper
functions, workflow wiring, and reporting integration used to exercise
the path publicly.

The current public outputs for those tests are the
**[code-flow artifact bundle][codeflow-bundle]** and the four published
recordings listed above. These are the human-reviewable scenario outputs
associated with the current public matrix.

This was not a generic share or invite setup reused for reporting. The
public suite defines a dedicated `code-flow` matrix, uses dedicated
Nextcloud and CERNBox code-flow wrappers, pins the milestone image tags
`sta-ocm-m6-debian` and `sta-ocm-m6-development`, and enables the
Reva-side exchanged-token topology through `OCM_ENABLE_CODE_FLOW=true`
together with the `OCMSHARECODE` and `OCMEXCHANGEDTOKEN` auth-provider
services. In other words, the published artifacts come from a runtime
that was configured specifically for the code-flow path.

The runtime path behind those tests is described in the paired
implementation work in **[cs3org/reva#5552][reva-5552]** and
**[enriquepablo/server#2][server-2]**. On the Reva side, that includes
discovery and token-exchange support together with the supporting fixes
needed for real interoperability. On the Nextcloud side, it includes
exchanged-token persistence, expiry-aware refresh handling, and the
remote-detection adjustments used in the remote-access flow.

The milestone-specific image and environment wiring in
**[MahdiBaghbani/containers#2][containers-2]** is part of the same
delivery chain, since it is the layer that made the milestone image
line available to the public test setup.

[sta-16]: https://github.com/cs3org/OCM-STA/issues/16
[ocmts-216]: https://github.com/cs3org/ocm-test-suite/pull/216
[run-111]: https://github.com/cs3org/ocm-test-suite/actions/runs/23564875382
[matrix]: https://github.com/cs3org/ocm-test-suite/blob/main/docs/4.2-compatibility-matrix.md
[site]: https://cs3org.github.io/ocm-test-suite/
[codeflow-bundle]: https://cs3org.github.io/ocm-test-suite/artifacts/bundles/ocm-tests-code-flow.zip
[containers-2]: https://github.com/MahdiBaghbani/containers/pull/2
[reva-5552]: https://github.com/cs3org/reva/pull/5552
[server-2]: https://github.com/enriquepablo/server/pull/2
[nextcloud-57234]: https://github.com/nextcloud/server/pull/57234
[platform-matrix-config]: https://github.com/cs3org/ocm-test-suite/blob/main/scripts/github/config/platforms.yaml
[nextcloud-codeflow-wrapper]: https://github.com/cs3org/ocm-test-suite/blob/main/scripts/utils/container/nextcloud_code_flow.sh
[cernbox-codeflow-wrapper]: https://github.com/cs3org/ocm-test-suite/blob/main/scripts/utils/container/cernbox.sh
[nextcloud-pull-script]: https://github.com/cs3org/ocm-test-suite/blob/main/docker/pull/ocm-test-suite/nextcloud.sh
[cernbox-pull-script]: https://github.com/cs3org/ocm-test-suite/blob/main/docker/pull/ocm-test-suite/cernbox.sh
[run-crnbx-crnbx]: https://github.com/cs3org/ocm-test-suite/actions/runs/23564880995
[run-crnbx-nc]: https://github.com/cs3org/ocm-test-suite/actions/runs/23564881070
[run-nc-crnbx]: https://github.com/cs3org/ocm-test-suite/actions/runs/23564881023
[run-nc-nc]: https://github.com/cs3org/ocm-test-suite/actions/runs/23564881032
[nextcloud-branch]: https://github.com/MahdiBaghbani/server/tree/sta-milestone-6/ocm-code-flow
[reva-branch]: https://github.com/MahdiBaghbani/reva-opencloud/tree/sta-milestone-6/ocm-code-flow
[contacts-branch]: https://github.com/MahdiBaghbani/nextcloud-contacts/tree/mahdi/fix/ui-optional-email
[video-crnbx-crnbx]: https://cs3org.github.io/ocm-test-suite/artifacts/code-flow-crnbx-v2-crnbx-v2/recording.mp4
[video-crnbx-nc]: https://cs3org.github.io/ocm-test-suite/artifacts/code-flow-crnbx-v2-nc-v33/recording.mp4
[video-nc-crnbx]: https://cs3org.github.io/ocm-test-suite/artifacts/code-flow-nc-v33-crnbx-v2/recording.mp4
[video-nc-nc]: https://cs3org.github.io/ocm-test-suite/artifacts/code-flow-nc-v33-nc-v33/recording.mp4
