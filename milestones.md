# Milestones

- Milestone M1: Directory Service and Where-Are-You-From UI in
  CERNBox
  - Implement WAYF UI with integration of one or more Directory
    Services, following the current Nextcloud developments
  - Conformance tests: extend Test Suite to validate that sharing
    between CERNBox and Nextcloud works also when a Directory
    Service lookup and a Where-are-you-from UI page is involved.

- Milestone M2: Plugin for running the OCM Test Suite in
  Nextcloud's and Reva’s CI pipelines
  - Produce and submit a PR upstream to Nextcloud and CERNBox,
    such that they can validate their compliance to the OCM spec
  - Parameterised matrix test for each Vendor V (V to/from Stub,
    V to/from V, Stub to/from Stub), where the “Stub” is an OCM
    stub implementation provided by
    https://github.com/cs3org/ocm-stub that is to be considered
    the reference implementation of the protocol.
  - Template documentation for each Vendor

- Milestone M3: Code-flow implementation in Nextcloud
  - Extend Nextcloud’s authentication logic to cover the code
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

| Milestone    | Deadline   | 
|--------------|------------|
| Milestone 1  | 2025-12-31 |
| Milestone 2  | 2025-12-31 |
| Milestone 3  | 2025-12-31 |
| Milestone 4  | 2025-12-31 |
| Milestone 5  | 2026-02-28 |
| Milestone 6  | 2026-02-28 |
| Milestone 7  | 2026-03-31 |
| Milestone 8  | 2026-05-31 |
| Milestone 9  | 2026-06-30 |
| Milestone 10 | 2026-08-30 |

