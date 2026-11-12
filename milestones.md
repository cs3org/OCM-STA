# Milestones

- Milestone M1: Directory Service and Where-Are-You-From UI in
  CERNBox
  - Implement WAYF UI with integration of one or more Directory
    Services, following the current Nextcloud developments
  - Conformance tests: extend Test Suite to validate that sharing
    between CERNBox and Nextcloud works also when a Directory
    Service lookup and a Where-are-you-from UI page is involved.

- Milestone M2: Plugin for running the OCM Test Suite in
  Nextcloud's and Reva's CI pipelines
  - Produce and submit a PR upstream to Nextcloud and CERNBox,
    such that they can validate their compliance to the OCM spec
  - Parameterised matrix test for each Vendor V (V to/from Stub,
    V to/from V, Stub to/from Stub), where the "Stub" is an OCM
    stub implementation provided by
    <https://github.com/cs3org/ocm-stub> that is to be considered
    the reference implementation of the protocol.
  - Template documentation for each Vendor

- Milestone M3: Code-flow implementation in Nextcloud
  - Extend Nextcloud's authentication logic to cover the code
    flow, by discovering the related capability
  - Add support for multi-protocol sharing in Nextcloud

- Milestone M4: Incorporate CERNBox share-flow test in the OCM
  Test Suite
  - Ensure full coverage for CERNBox in the OCM Test Suite (the
    assumption is that some tests do not pass)
  - Implement any necessary fix for the sharing phase to pass the
    OCM Test Suite

- Milestone M5: OCM Stub Implementation
  - Rewrite OCM stub in a compiled language
  - Implement capability discovery endpoint, loading
    certificates, ACME auto TLS
  - Implement better configurability
  - Implement Docker deploy
  - Implement full support in the OCM Test Suite

- Milestone M6: Code-flow validation between Nextcloud and
  CERNBox
  - Validate logic against CERNBox
  - Implement spec conformance tests: CERNBox to Nextcloud share
    round-trip with token exchange on remote access

- Milestone M7: Structured logging for test suite
  - Implement JSON logs per platform, artefact upload
  - Implement Log viewer in CI job summary

- Milestone M8: Application Federation
  - Implement applications federation in Nextcloud, following the
    CERNBox model and targeting in particular Jupyterhub,
    Nextcloud Talk, and Office-like web apps.
  - Validate Nextcloud to CERNBox application sharing and vice
    versa

- Milestone M9: Web-based Federation Validator
  - Building on the M5 deliverable, provide a JSON-level
    standalone validator endpoint for any implementer to test
    against
  - Add external probes (well-known, TLS, sign-key)
  - Deploy on CS3 GitHub Pages
  - Return graded report

- Milestone M10: OCM Standard Development
  - Incorporate in the IETF Internet Draft all suggested changes
    following implementations from all other Milestones
  - Attend the meetings of the WG being formed within IETF,
    follow their work and provide feedback.
  - Get all standardization-related documents in shape according
    to IETF deadlines in the course of 2026 and prepare for IETF
    126 in Vienna.

- Milestone M11: Notifications Re-haul
  - Align major implementations with the re-hauled Notifications
    specification to ensure interoperability.
  - OCM Notifications in CERNBox:
    - Provide the foundation and integration in the recently
      revamped CERNBox notifications system.
    - Support notifications for files' permission updates,
      incoming and outgoing.
    - Support notifications for removing users, incoming and
      outgoing.
  - OCM Notifications in Nextcloud:
    - Introduce payload-based logic to distinguish legacy
      Nextcloud-specific notifications from OCM-standard ones.
    - Add an option to disable third-party sharing and its
      corresponding legacy notifications, which are now out of
      spec.
  - OCM Notifications in OpenCloud:
    - Introduce payload-based logic to distinguish legacy
      OpenCloud-specific notifications from OCM-standard ones.
    - Reuse patterns from CERNBox where appropriate.

- Milestone M12: Proof of concept of MLS
  - Design and implement a proof of concept demonstrating the
    management of a federation of groups using Messaging Layer
    Security (MLS), specified in RFC 9420, following the new
    OCM-MLS draft.
  - Extend the reference implementation from Milestone 5,
    and/or CERNBox, and/or Nextcloud with support for OCM-MLS
    notifications.
  - Provide feedback on the correctness of the OCM-MLS
    specification.

- Milestone M13: OCM Standard Development
  - Continue Milestone 10 by incorporating in the Internet-Drafts
    all changes arising from implementation in Milestones 11
    and 12.
  - Incorporate all specification changes resulting from the
    Notifications Re-haul and MLS proof of concept.
  - Evolve the specifications from implementation lessons,
    focusing on newer sections not yet reflected in an actual
    implementation.

- Milestone M14: Distributed Directory Service Implementation
  - Following the approach of Milestones 3 and 8, implement the
    decentralized Directory Service and demonstrate it for the
    main cloud storage systems.
  - Implement a proof of concept in CERNBox and Nextcloud.
  - Explore extending the validator delivered in Milestone 9
    to validate the distributed Directory Service.
  - Where needed, amend the specification following the same
    workflow as Milestone 13.

| Milestone    | Deadline   |
|--------------|------------|
| Milestone 1  | 2025-12-31 |
| Milestone 2  | 2025-12-31 |
| Milestone 3  | 2025-12-31 |
| Milestone 4  | 2025-12-31 |
| Milestone 5  | 2026-02-28 |
| Milestone 6  | 2026-03-31 |
| Milestone 7  | 2026-04-30 |
| Milestone 8  | 2026-06-30 |
| Milestone 9  | 2026-08-31 |
| Milestone 10 | 2026-09-30 |
| Milestone 11 | 2026-11-30 |
| Milestone 12 | 2026-12-31 |
| Milestone 13 | 2026-12-31 |
| Milestone 14 | 2027-01-31 |

## Contract amendment effective 3 November 2026

The amendment extends the contract term to 31 January 2027 and
adds EUR 40,000 for Milestones 11-14. The overall remuneration
cap is EUR 140,000 plus statutory VAT. The hourly rate and other
remuneration terms remain unchanged.

| Milestone | Amount (EUR) |
|-----------|-------------:|
| M11       |       12,000 |
| M12       |        9,000 |
| M13       |       12,000 |
| M14       |        7,000 |
| Total     |       40,000 |
