# Milestone report: OCM Standard Development (M10)

This report covers the work carried out for Milestone M10 of the OCM-STA
project.

The milestone focuses on advancing the OCM specification, in collaboration
with the IETF Working Group and in coordination with the work carried
in the scope of all other Milestones, in order to validate the specification
with real-world implementations.

Milestone M10 is tracked in [cs3org/OCM-STA#25][sta-25] and
[cs3org/OCM-STA#42][sta-42].

The contractual milestone deadline in the upstream milestone list is
2026-08-31.

## Executive summary

Milestone M10 spanned a large period of time, deliberately overlapping with
most other Milestones, in order to close the loop between the protocol
specification development and the actual implementations, thus validating
that the protocol specification is viable and secure.

## Contractual milestone definition

According to the project milestone list, Milestone M10 is defined as:

- Incorporate in the IETF Internet Draft all suggested changes following
  implementations from all other Milestones.
- Attend the meetings of the WG being formed within IETF, follow their work
  and provide feedback.
- Get all standardization-related documents in shape according to IETF
  deadlines in the course of 2026 and prepare for IETF 126 in Vienna.

Deadline: 2026-08-31

## Work performed

### 1. Specification Development

The development of the protocol specification involved a significant number
of amendments to the Internet-Draft text. The list of all amendments is
referenced in [cs3org/OCM-STA#25][sta-25] with their Pull Request links.

The changes resulted in seven published Versions of the core OCM IETF
[Internet-Draft][draft-ocm], along with the creation of two additional IETF
Internet-Drafts:

- The [OCM Integration Protocol I-D][draft-ocm-ip], at Version 02, to support
  the integration of third-party applications such as JupyterLab.
- The [OCM Message Layer Security I-D][draft-ocm-mls], at Version 03, to
  provide a specification useful to implement federated groups spanning multiple
  institutions, a concept that was previously only mentioned but not described
  in details.

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
  - Adds the Open Cloud Mesh Message Layer Security draft used to support
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

### 2. Participation to the IETF meetings

At least one team member has participated in all IETF meetings since the
beginning of the project, including IETF 124 in Montreal and IETF 125 in
Shenzhen. On both occasions, lightning talks about OCM were also
presented. The working group held its first
[interim, on-line meeting][ietf-interim] meeting in November 2025 and a
second one in April 2026, in preparation for the
[in-person meeting][ietf-meeting] held at IETF 126 in Vienna in July 2026.

In addition, regular bi-weekly on-line meetings, as well as other informal
meetings, have taken place throughout the year in order to discuss and
resolve the issues related to the Internet Drafts and the implementations.

### 3. Preparation for the IETF 126 meeting in Vienna

Multiple presentations were prepared for the in-person IETF 126 meeting.
These have all been published in the [session agenda][ietf-meeting].

Furthermore, the Internet Drafts were reviewed by IETF IANA members, and
contacts were established with other IETF experts in related technologies
mentioned in the specification, such as MLS and HTTP message signatures,
in order to validate assumptions in the specification, or improve it where
necessary.

## Results vs milestone goals

### Goal 1: Advance the OCM Specification to incorporate all relevant input

Version 07 of the core OCM Internet Draft incorporates all input and feedback
collected so far wthin and outside the IETF. Additional Internet Drafts stand
on their own to develop specific aspects of the protocol.

### Goal 2: Prepare for and participate to the IETF 126 meeting

In-person participation of two team members and co-authors of the Internet
Drafts was ensured, with a number of presentations as detailed in the public
session agenda hosted by the [IETF][ietf-meeting].

--

[sta-25]: https://github.com/cs3org/OCM-STA/issues/25
[sta-42]: https://github.com/cs3org/OCM-STA/issues/42
[openapi-spec]: https://cs3org.github.io/OCM-API/docs.html?tag=v1.5.0&repo=OCM-API&user=cs3org
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
[draft-ocm-ip]: https://datatracker.ietf.org/doc/draft-nordin-ocm-integration-protocol
[draft-ocm-mls]: https://datatracker.ietf.org/doc/draft-nordin-ocm-mls-federated-groups
