# Milestone report: Web-based Federation Validator (M9)

This report covers the work carried out for Milestone M9 of the OCM-STA
project. The milestone builds on the M5 deliverable to provide a JSON-level
standalone validator endpoint that any implementer can test against, adds
external probes for well-known discovery, TLS, and signing keys, deploys the
validator publicly, and returns a graded report.

Milestone M9 is tracked in [cs3org/OCM-STA#21][sta-21]. The public record spans
the validator backend and JSON-level endpoint in `opencloudmesh-go`, the
external probe suite, the validator web interface in `ocm-web-site` and its
deployment together with the backend, the supporting build and test-suite work,
and the ecosystem OCM fixes that the active validation depends on. The evidence
comment on
[cs3org/OCM-STA#21][sta-21] collects the passive and active walk-through
screenshots referenced below.

The contractual milestone deadline in the upstream milestone list is
2026-08-31.

## Executive summary

Milestone M9 builds on the M5 Go OCM stub to add a compatibility validator. The
`opencloudmesh-go` service exposes a JSON-level validator endpoint that takes a
target host and returns a graded, evidence-backed compatibility report, so an
implementer can test their server without a local checkout.

The validator runs in two modes. The passive mode needs no account on the
target and probes the external surface: well-known OCM discovery, TLS, signing
keys (JWKS), and an HTTP-signature check. The active mode drives a full OCM flow
against a real account on the target: invitation, accept, return invitation,
forward share, opening the shared file, and a reverse share back to the
validator. Both modes grade each area as pass, warn, or fail and publish the
full per-area breakdown rather than stopping at the first problem.

The interface and its backend are hosted together on a VPS, and the graded
report comes back both as JSON from the endpoint and as a rendered report in the
interface. I ran the passive and active passes against a live target; the
walk-through and screenshots are attached to
[cs3org/OCM-STA#21][sta-21]. The per-area grades are there to show the validator
producing evidence-backed reports, not to pass or fail the tested peer.

## Contractual milestone definition

According to the project milestone list, Milestone M9 is defined as:

- Milestone M9: Web-based Federation Validator
  - Building on the M5 deliverable, provide a JSON-level standalone validator
    endpoint for any implementer to test against
  - Add external probes (well-known, TLS, sign-key)
  - Deploy on CS3 GitHub Pages
  - Return graded report

Deadline: 2026-08-31

## Work performed

### 1. Milestone tracker and coordination

- **[cs3org/OCM-STA#21][sta-21]** - Building on the M5 deliverable, provide a
  JSON-level standalone validator endpoint for any implementer to test against.
  - Serves as the milestone tracking entry point.
  - Its comments collect the implementation pull requests and the passive and
    active evidence walk-through used below.

### 2. Validator backend and JSON-level endpoint

The validator backend extends the M5 Go OCM stub
([MahdiBaghbani/opencloudmesh-go@v1.0.0][ocmgo-v1]) with a standalone
validator endpoint. The endpoint accepts a target host, runs the probe suite or
the active flow, and returns a graded JSON report.

- **[MahdiBaghbani/opencloudmesh-go#89][ocmgo-89]** (merged)
- **[MahdiBaghbani/opencloudmesh-go#90][ocmgo-90]** (merged)
- **[MahdiBaghbani/opencloudmesh-go#93][ocmgo-93]** (merged)
- **[MahdiBaghbani/opencloudmesh-go#94][ocmgo-94]** (merged)
  - Together these add the validator core, the JSON-level endpoint, the session
    model behind the active flow, and the graded report envelope.

### 3. External probes (well-known, TLS, sign-key)

The passive mode implements the contract's external probes. Each probe grades
its area and records the evidence behind the grade.

- Well-known discovery: an uncached `GET /.well-known/ocm` that checks the
  discovery document for `enabled`, `apiVersion`, `endPoint`, and
  `resourceTypes`, and warns when the peer advertises a different OCM apiVersion
  than the validator pins.
- TLS: a transport probe over the target's HTTPS endpoint.
- Signing keys: a JWKS probe that checks whether the peer advertises signing
  keys, plus an HTTP-signature check for inbound RFC 9421 verification.

The probe suite ships in the `opencloudmesh-go` validator work referenced in
section 2 and is exercised in the passive evidence in the live demonstration
section below.

### 4. Validator web interface and deployment

The validator user interface is part of the OCM website (`ocm-web-site`). It
gives you the "Check a server" entry form, the passive and active run views, the
per-area result cards with drill-down evidence, and the graded report view.

The graded validator needs the `opencloudmesh-go` backend behind it to run the
probes and the active flow, so it can't live as static files alone. GitHub Pages
only serves static content, and a browser-only design that called peers directly
would hit CORS and other cross-origin limits. My preference was to keep the
interface and its backend together, so I'm hosting both on a VPS at
**[ocm.azadehafzar.io][validator]**.

The OCM Test Suite matrix and CI results (the Observatory) still publish on
GitHub Pages through GitHub Actions at
**[cs3org.github.io/ocm-test-suite][observatory]**. Now that ownCloud has
sponsored us the `ocm.org` domain, the plan is to bring the validator and the
Observatory together under one dedicated home.

- **[MahdiBaghbani/ocm-web-site#8][site-8]** (merged)
- **[MahdiBaghbani/ocm-web-site#9][site-9]** (merged)
- **[MahdiBaghbani/ocm-web-site#10][site-10]** (merged)
- **[MahdiBaghbani/ocm-web-site#11][site-11]** (merged)
- **[MahdiBaghbani/ocm-web-site#12][site-12]** (merged)
- **[MahdiBaghbani/ocm-web-site#13][site-13]** (merged)

### 5. Build, packaging, and test coverage

- **[MahdiBaghbani/opencloudmesh-go#24][ocmgo-24]** (merged)
  - Playwright end-to-end wiring for the validator API and UI flows.
- **[MahdiBaghbani/opencloudmesh-go#38][ocmgo-38]** (merged)
  - Pure-Go SQLite static binary so the validator deploys as a single
    self-contained artifact.
- **[cs3org/ocm-test-suite#215][ocmts-215]** (merged),
  **[cs3org/ocm-test-suite#219][ocmts-219]** (merged), and
  **[cs3org/ocm-test-suite#220][ocmts-220]** (merged)
  - The `opencloudmesh-go` cells and coverage in the shared OCM Test Suite.

### 6. Ecosystem OCM fixes enabling the active validation

The active validation drives a full OCM flow against a real peer. The following
upstream fixes were needed so that flow completes end to end. They live in other
projects and are listed as supporting work rather than as M9 deliverables.

- **[cs3org/reva#5415][reva-5415]** (merged),
  **[cs3org/reva#5418][reva-5418]** (merged),
  **[cs3org/reva#5552][reva-5552]** (merged),
  **[cs3org/reva#5695][reva-5695]** (merged),
  **[cs3org/reva#5696][reva-5696]** (merged), and
  **[cs3org/reva#5698][reva-5698]** (merged)
  - Invite-accept user typing, invite expiry handling, code-flow token
    exchange, and OCM user-identifier and single-file WebDAV share handling.
- **[opencloud-eu/opencloud#2404][opencloud-2404]** (merged)
  - Discovery and TLS handling on the OpenCloud side.
- **[SUNET/nextcloud-integration_jupyterhub#23][jupyterhub-23]** (merged)
  - WebDAV resource-type registration alongside WebApp support.

A few related upstream fixes are still open at the time of writing and are noted
for completeness, not counted as delivered evidence:
**[cs3org/reva#5701][reva-5701]**, **[owncloud/ocis#11758][ocis-11758]**,
**[owncloud/reva#432][owncloud-reva-432]**,
**[cernbox/web#228][cernbox-228]**, and **[cernbox/web#260][cernbox-260]**.

## Specification alignment

The validator grades against the OCM-API specification. The following
specification pull requests are included as technical context; their formal
reporting remains with M10.

- **[cs3org/OCM-API#317][ocm-api-317]** (merged)
  - Discovery schema and HTTP-signature naming.
- **[cs3org/OCM-API#354][ocm-api-354]** (merged)
  - Code-flow sender and receiver semantics.
- **[cs3org/OCM-API#395][ocm-api-395]** (merged)
  - JWKS advertisement via the discovery `jwksUri`.

## Live demonstration and evidence

The passive and active runs were carried out against a live target, and the
graded reports came out of the validator. The full walk-through, with a title
and a short note for each screenshot, is attached to
[cs3org/OCM-STA#21][sta-21].

### Passive run

The passive report grades the four no-account areas. In the demonstrated run,
secure connection passed, server discovery and signing keys warned, and request
signing failed, while the active-only areas stayed untested. The discovery
warning was the advertised OCM apiVersion differing from the version the
validator pins; the signing-keys warning was the peer not advertising a JWKS
URL. The validator published the full graded report rather than stopping at the
first problem.

### Active run

The active report covers the full OCM flow: creating and accepting an
invitation in both directions, a forward share that arrives and opens on the
target, and a reverse share created on the target and observed by the validator.
In the demonstrated run, share exchange and capabilities passed, discovery and
signing keys warned, and request signing failed for the same reasons as the
passive run. This exercises and grades the live invite, accept, forward-share,
file-open, and reverse-share exchange.

As with the M7 reporting surface, the demonstrated grades are here to show the
validator producing evidence-backed reports, not to pass or fail the tested peer.

## Work mapped to milestone goals

### Goal 1: Building on the M5 deliverable, provide a JSON-level standalone validator endpoint

The record for this goal is the validator backend in
**[opencloudmesh-go#89][ocmgo-89]**, **[#90][ocmgo-90]**, **[#93][ocmgo-93]**,
and **[#94][ocmgo-94]**, which extends the M5 Go OCM stub ([v1.0.0][ocmgo-v1])
with a standalone endpoint that takes a target host and returns a graded JSON
report, so any implementer can test against it.

### Goal 2: Add external probes (well-known, TLS, sign-key)

The passive mode covers this one: the well-known discovery probe, the TLS probe,
and the signing-key probes (JWKS plus HTTP-signature), each grading its area with
recorded evidence. The passive evidence in [cs3org/OCM-STA#21][sta-21] shows
these probes running and grading.

### Goal 3: Deploy on CS3 GitHub Pages

The interface ships through **[ocm-web-site#8][site-8]** to **[#13][site-13]**.
The graded validator needs the `opencloudmesh-go` backend behind it, and GitHub
Pages only serves static content, so I'm hosting the interface and backend
together on a VPS at **[ocm.azadehafzar.io][validator]** instead of on GitHub
Pages. A browser-only design would also have run into CORS and other
cross-origin limits. The OCM Test Suite matrix and CI results still publish on
GitHub Pages at **[cs3org.github.io/ocm-test-suite][observatory]**, and now that
ownCloud has sponsored the `opencloudmesh.org` domain, the plan is to bring these
together under one home.

So this goal landed a bit differently from the original GitHub Pages plan: the
validator is public and live, just hosted on the VPS rather than on Pages. I'm
noting that here so you can judge it as it is.

### Goal 4: Return graded report

The graded report comes back both as JSON from the endpoint and as a rendered
report in the interface, with per-area pass, warn, or fail grades and drill-down
evidence. The passive and active graded reports are demonstrated in
[cs3org/OCM-STA#21][sta-21].

[sta-21]: https://github.com/cs3org/OCM-STA/issues/21
[ocmgo-v1]: https://github.com/MahdiBaghbani/opencloudmesh-go/tree/v1.0.0
[ocmgo-89]: https://github.com/MahdiBaghbani/opencloudmesh-go/pull/89
[ocmgo-90]: https://github.com/MahdiBaghbani/opencloudmesh-go/pull/90
[ocmgo-93]: https://github.com/MahdiBaghbani/opencloudmesh-go/pull/93
[ocmgo-94]: https://github.com/MahdiBaghbani/opencloudmesh-go/pull/94
[ocmgo-24]: https://github.com/MahdiBaghbani/opencloudmesh-go/pull/24
[ocmgo-38]: https://github.com/MahdiBaghbani/opencloudmesh-go/pull/38
[site-8]: https://github.com/MahdiBaghbani/ocm-web-site/pull/8
[site-9]: https://github.com/MahdiBaghbani/ocm-web-site/pull/9
[site-10]: https://github.com/MahdiBaghbani/ocm-web-site/pull/10
[site-11]: https://github.com/MahdiBaghbani/ocm-web-site/pull/11
[site-12]: https://github.com/MahdiBaghbani/ocm-web-site/pull/12
[site-13]: https://github.com/MahdiBaghbani/ocm-web-site/pull/13
[ocmts-215]: https://github.com/cs3org/ocm-test-suite/pull/215
[ocmts-219]: https://github.com/cs3org/ocm-test-suite/pull/219
[ocmts-220]: https://github.com/cs3org/ocm-test-suite/pull/220
[reva-5415]: https://github.com/cs3org/reva/pull/5415
[reva-5418]: https://github.com/cs3org/reva/pull/5418
[reva-5552]: https://github.com/cs3org/reva/pull/5552
[reva-5695]: https://github.com/cs3org/reva/pull/5695
[reva-5696]: https://github.com/cs3org/reva/pull/5696
[reva-5698]: https://github.com/cs3org/reva/pull/5698
[reva-5701]: https://github.com/cs3org/reva/pull/5701
[opencloud-2404]: https://github.com/opencloud-eu/opencloud/pull/2404
[jupyterhub-23]: https://github.com/SUNET/nextcloud-integration_jupyterhub/pull/23
[ocis-11758]: https://github.com/owncloud/ocis/pull/11758
[owncloud-reva-432]: https://github.com/owncloud/reva/pull/432
[cernbox-228]: https://github.com/cernbox/web/pull/228
[cernbox-260]: https://github.com/cernbox/web/pull/260
[ocm-api-317]: https://github.com/cs3org/OCM-API/pull/317
[ocm-api-354]: https://github.com/cs3org/OCM-API/pull/354
[ocm-api-395]: https://github.com/cs3org/OCM-API/pull/395
[validator]: https://ocm.azadehafzar.io/validator/
[observatory]: https://cs3org.github.io/ocm-test-suite/
