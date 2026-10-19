# Milestone report: OCM Standard Development (M10)

This report covers the work carried out for Milestone M10 of the OCM-STA
project.

The milestone focuses on advancing the OCM specification, in collaboration
with the IETF Working Group and in coordination with the work carried
out in the scope of all other Milestones, in order to validate the specification
with real-world implementations.

Milestone M10 is tracked in [cs3org/OCM-STA#25][sta-25],
[#26][sta-26], [#27][sta-27], and [#42][sta-42].

The contractual milestone deadline in the upstream milestone list is
2026-09-30.

## Executive summary

Milestone M10 spanned a large period of time, deliberately overlapping with
most other Milestones, in order to close the loop between the protocol
specification development and the actual implementations, helping to validate
that the protocol specification is viable and secure.

Beyond the contractual goals, the M10 period was also used to carry earlier
milestone work further: OCM security hardening in Reva, WebApp sharing on the
CERNBox side, the supporting test-suite and image fixes, and a common web
home for OCM. This is summarized in
[Additional work during the M10 period](#additional-work-during-the-m10-period).

## Contractual milestone definition

According to the project milestone list, Milestone M10 is defined as:

- Incorporate in the IETF Internet Draft all suggested changes following
  implementations from all other Milestones.
- Attend the meetings of the WG being formed within IETF, follow their work
  and provide feedback.
- Get all standardization-related documents in shape according to IETF
  deadlines in the course of 2026 and prepare for IETF 126 in Vienna.

Deadline: 2026-09-30

## Work performed

### 1. Specification Development

The development of the protocol specification involved a significant number
of amendments to the Internet-Draft text. The list of all amendments is
referenced in [cs3org/OCM-STA#25][sta-25] with their Pull Request links.

The changes resulted in revisions through Version 07 of the core OCM IETF
[Internet-Draft][draft-ocm], along with the creation of two additional IETF
Internet-Drafts, both adopted by the IETF WG:

- The [OCM Integration Protocol I-D][draft-ocm-ip], at Version 00, to support
  the integration of third-party applications such as JupyterLab.
- The [OCM Messaging Layer Security I-D][draft-ocm-mls], at Version 00, to
  provide a specification useful to implement federated groups spanning multiple
  institutions, a concept that was previously only mentioned but not described
  in detail.

The work included keeping the OpenAPI specification of the protocol up to date,
which resulted in the delivery of [version 1.5.0][openapi-spec], latest at the time of
writing this report.

Here follows a non-exhaustive list of the most relevant changes:

- **[cs3org/OCM-API#318][ocm-api-318]**
  - Add JSContact extension to IANA Considerations.
- **[cs3org/OCM-API#354][ocm-api-354]**
  - Clarifies Token exchange, known as Code flow, sender and receiver
    semantics.
- **[cs3org/OCM-API#368][ocm-api-368]**
  - Updates WebApp discovery, permissions, targets, shared-secret handling,
    and the WebApp share schema.
- **[cs3org/OCM-API#370][ocm-api-370]**
  - Adds the Open Cloud Mesh Integration Protocol draft used for
    protocol-server integration, including web application platforms.
- **[cs3org/OCM-API#372][ocm-api-372]**
  - Adds the Open Cloud Mesh Messaging Layer Security draft used to support
    federated groups spanning multiple institutions.
- **[cs3org/OCM-API#380][ocm-api-380]**
  - Introduces IANA Registries to make the OCM protocol extendable.
- **[cs3org/OCM-API#391][ocm-api-391]**
  - Addressed comments from IETF HTTP Sig reviewers.
- **[cs3org/OCM-API#400][ocm-api-400]**
  - Updates the Notifications and adds a full specification of the allowed
    payloads, whilst allowing for custom extensions.
- **[cs3org/OCM-API#402][ocm-api-402]**
  - Move encryption to the main I-D.

### 2. Participation in the IETF meetings

At least one team member has participated in all IETF meetings since the
beginning of the project, including IETF 124 in Montreal and IETF 125 in
Shenzhen. On both occasions, lightning talks about OCM were also
presented. The working group held its first
[interim, on-line meeting][ietf-interim] in November 2025 and a
second one in April 2026, in preparation for the
[in-person meeting][ietf-meeting] held at IETF 126 in Vienna in July 2026.

In addition, regular bi-weekly on-line meetings, as well as other informal
meetings, have taken place throughout the year in order to discuss and
resolve the issues related to the Internet Drafts and the implementations.

### 3. Preparation for the IETF 126 meeting in Vienna

Multiple presentations were prepared for the in-person IETF 126 meeting.
These have all been published in the [session agenda][ietf-meeting].

Furthermore, the Internet Drafts were reviewed by IANA reviewers, and
contacts were established with other IETF experts in related technologies
mentioned in the specification, such as MLS and HTTP message signatures,
in order to validate assumptions in the specification, or improve it where
necessary.

## Results vs milestone goals

### Goal 1: Advance the OCM Specification to incorporate all relevant input

Version 07 of the core OCM Internet Draft incorporates all input and feedback
collected so far within and outside the IETF. Additional Internet Drafts stand
on their own to develop specific aspects of the protocol.

### Goal 2: Prepare for and participate in the IETF 126 meeting

In-person participation of two team members and co-authors of the Internet
Drafts was ensured, with a number of presentations as detailed in the public
session agenda hosted by the [IETF][ietf-meeting].

## Additional work during the M10 period

The following work is not part of the M10 definition. It continues items left
open in earlier milestone reports and is listed here so that it is on record.
The full pull request inventory is kept in [cs3org/OCM-STA#48][sta-48].

### OCM security hardening in Reva (continues M1 and M9)

- **[cs3org/reva#5833][reva-5833]** (merged), with
  **[#5834][reva-5834]**, **[#5836][reva-5836]**, **[#5837][reva-5837]**,
  **[#5838][reva-5838]**, and **[#5839][reva-5839]** (included in master
  manually; landing commits listed in [cs3org/OCM-STA#48][sta-48])
  - A shared OCM HTTP transport with TLS and scheme policy, bounded
    peer-controlled JSON, and guarded discovery and receiver inputs.
  - Supersedes **[cs3org/reva#5389][reva-5389]**, recorded as volunteer
    follow-up work in the M1 report.

### WebApp sharing on CERNBox (continues M8)

- **[cs3org/reva#5850][reva-5850]** to **[#5856][reva-5856]** and
  **[#5885][reva-5885]** (open)
  - Safe launch of received WebApp shares, exposed WebApp metadata,
    validated WebApp requirements, outbound WebApp offers behind a
    configuration flag, and token-exchange fixes.
- **[cernbox/web#274][cernbox-274]**, **[#275][cernbox-275]**, and
  **[#276][cernbox-276]** (open)
  - Carry the WebApp name through received shares and gate remote WebApp
    actions by protocol.
- **[cs3org/reva-configs#21][reva-configs-21]** (merged) and
  **[#22][reva-configs-22]** (open)
  - CERNBox deployment configuration for the new Reva services.
- Supersedes **[cs3org/reva#5701][reva-5701]** and
  **[cernbox/web#260][cernbox-260]**, listed as open in the M8 and M9 reports.

### Test suite, images, and evidence

- **[cs3org/ocm-test-suite#218][ocmts-218]**, **[#228][ocmts-228]**,
  **[#230][ocmts-230]**, **[#231][ocmts-231]**, and **[#233][ocmts-233]** to
  **[#235][ocmts-235]** (merged)
  - CI workflow and WebApp flow consolidation, MITM and launch-probe fixes,
    repaired contact and share flows, and refreshed Nextcloud images.
- **[MahdiBaghbani/containers#22][containers-22]** to
  **[#25][containers-25]** (merged) and
  **[SUNET/nextcloud-integration_jupyterhub#25][jupyterhub-25]** (merged)
  - Nextcloud v35.0.1 and Reva image updates for the WebApp share line.
- In **[GitHub Actions run 38032570872][run-38032570872]**, both WebApp Share
  cells, Nextcloud v35 -> CERNBox v11 and Nextcloud v35 -> Nextcloud v35,
  completed successfully.

### Web presence

- **[cs3org/ocm-test-suite#229][ocmts-229]** and
  **[MahdiBaghbani/ocm-web-site#14][site-14]** to **[#18][site-18]** (merged)
  - The Observatory is now the root of
    [cs3org.github.io/ocm-test-suite][observatory], and the OCM site and the
    [validator][validator] link to the [OCM community home][ocm-home].

### On hold

- **[cs3org/reva#5458][reva-5458]** (draft, on hold)
  - The CERNBox OCM Test Suite CI workflow from M2 is suspended and not yet
    finished.
- SUNET hosting of the validator and MyAccessID support are pending
  deployment work, with no pull requests yet.

--

[sta-25]: https://github.com/cs3org/OCM-STA/issues/25
[sta-26]: https://github.com/cs3org/OCM-STA/issues/26
[sta-27]: https://github.com/cs3org/OCM-STA/issues/27
[sta-42]: https://github.com/cs3org/OCM-STA/issues/42
[openapi-spec]: https://cs3org.github.io/OCM-API/docs.html?branch=v1.5.0&repo=OCM-API&user=cs3org
[ocm-api-318]: https://github.com/cs3org/OCM-API/pull/318
[ocm-api-354]: https://github.com/cs3org/OCM-API/pull/354
[ocm-api-368]: https://github.com/cs3org/OCM-API/pull/368
[ocm-api-370]: https://github.com/cs3org/OCM-API/pull/370
[ocm-api-372]: https://github.com/cs3org/OCM-API/pull/372
[ocm-api-380]: https://github.com/cs3org/OCM-API/pull/380
[ocm-api-391]: https://github.com/cs3org/OCM-API/pull/391
[ocm-api-400]: https://github.com/cs3org/OCM-API/pull/400
[ocm-api-402]: https://github.com/cs3org/OCM-API/pull/402
[ietf-interim]: https://datatracker.ietf.org/meeting/interim-2025-ocm-01/session/ocm
[ietf-meeting]: https://datatracker.ietf.org/meeting/126/session/ocm
[draft-ocm]: https://datatracker.ietf.org/doc/draft-ietf-ocm-open-cloud-mesh
[draft-ocm-ip]: https://datatracker.ietf.org/doc/draft-ietf-ocm-integration-protocol
[draft-ocm-mls]: https://datatracker.ietf.org/doc/draft-ietf-ocm-mls-federated-groups
[reva-5389]: https://github.com/cs3org/reva/pull/5389
[reva-5458]: https://github.com/cs3org/reva/pull/5458
[reva-5701]: https://github.com/cs3org/reva/pull/5701
[reva-5833]: https://github.com/cs3org/reva/pull/5833
[reva-5834]: https://github.com/cs3org/reva/pull/5834
[reva-5836]: https://github.com/cs3org/reva/pull/5836
[reva-5837]: https://github.com/cs3org/reva/pull/5837
[reva-5838]: https://github.com/cs3org/reva/pull/5838
[reva-5839]: https://github.com/cs3org/reva/pull/5839
[reva-5850]: https://github.com/cs3org/reva/pull/5850
[reva-5856]: https://github.com/cs3org/reva/pull/5856
[reva-5885]: https://github.com/cs3org/reva/pull/5885
[reva-configs-21]: https://github.com/cs3org/reva-configs/pull/21
[reva-configs-22]: https://github.com/cs3org/reva-configs/pull/22
[cernbox-260]: https://github.com/cernbox/web/pull/260
[cernbox-274]: https://github.com/cernbox/web/pull/274
[cernbox-275]: https://github.com/cernbox/web/pull/275
[cernbox-276]: https://github.com/cernbox/web/pull/276
[ocmts-218]: https://github.com/cs3org/ocm-test-suite/pull/218
[ocmts-228]: https://github.com/cs3org/ocm-test-suite/pull/228
[ocmts-229]: https://github.com/cs3org/ocm-test-suite/pull/229
[ocmts-230]: https://github.com/cs3org/ocm-test-suite/pull/230
[ocmts-231]: https://github.com/cs3org/ocm-test-suite/pull/231
[ocmts-233]: https://github.com/cs3org/ocm-test-suite/pull/233
[ocmts-235]: https://github.com/cs3org/ocm-test-suite/pull/235
[containers-22]: https://github.com/MahdiBaghbani/containers/pull/22
[containers-25]: https://github.com/MahdiBaghbani/containers/pull/25
[jupyterhub-25]: https://github.com/SUNET/nextcloud-integration_jupyterhub/pull/25
[site-14]: https://github.com/MahdiBaghbani/ocm-web-site/pull/14
[site-18]: https://github.com/MahdiBaghbani/ocm-web-site/pull/18
[run-38032570872]: https://github.com/cs3org/ocm-test-suite/actions/runs/38032570872
[observatory]: https://cs3org.github.io/ocm-test-suite/
[validator]: https://ocm.azadehafzar.io/validator/
[ocm-home]: https://www.cs3community.org/ocm
