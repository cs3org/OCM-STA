# Milestone report: M5 (OCM Stub Implementation)

This report documents work delivered for Milestone M5 (OCM Stub Implementation) under the SovereignTech Funded Activities project. Reporting period: 2026-01-01 through 2026-02-15. Milestone deadline: 2026-02-28.

## Executive summary

The OCM stub was rewritten in Go and tagged as `v1.0.0` in `opencloudmesh-go`. Docker deployment and OCM Test Suite E2E coverage were added. The repo also has its own test suite: unit, integration, and Playwright E2E for API and UI flows.

Key deliverables:

- Core implementation and deliverable tag: [MahdiBaghbani/opencloudmesh-go@v1.0.0](https://github.com/MahdiBaghbani/opencloudmesh-go/tree/v1.0.0)
- Docker deployment workflow: [MahdiBaghbani/opencloudmesh-go#1](https://github.com/MahdiBaghbani/opencloudmesh-go/commit/b3fd9d95352a21e743ef73210462f580507e14ee)
- OCM Test Suite E2E coverage: [cs3org/ocm-test-suite#215](https://github.com/cs3org/ocm-test-suite/pull/215)
- OCM-API reference specification (discovery): [cs3org/OCM-API IETF-RFC (discovery)](https://github.com/cs3org/OCM-API/blob/615192eeff00bcd479364dfa9c1f91641ac7b505/IETF-RFC.md?plain=1#ocm-api-discovery)

## Contractual milestone definition

Milestone M5: OCM Stub Implementation (deadline: 2026-02-28)

- Rewrite OCM stub in a compiled language
- Implement capability discovery endpoint, loading certificates, ACME auto TLS
- Implement better configurability
- Implement Docker deploy
- Implement full support in the OCM Test Suite

## Work performed

### Core OCM stub implementation (compiled language)

- Go-based OCM stub in [MahdiBaghbani/opencloudmesh-go@v1.0.0](https://github.com/MahdiBaghbani/opencloudmesh-go/tree/v1.0.0). Signed, annotated tag; main milestone deliverable.
- Work tracking issues in the milestone tracker repository:
  - [cs3org/OCM-STA#10](https://github.com/cs3org/OCM-STA/issues/10)
  - [cs3org/OCM-STA#11](https://github.com/cs3org/OCM-STA/issues/11)
  - [cs3org/OCM-STA#12](https://github.com/cs3org/OCM-STA/issues/12)

### Protocol alignment with the OCM-API specification

The OCM protocol surface can be verified directly through the discovery document and the referenced wire-format definitions.

- The server exposes the OCM discovery document at `/.well-known/ocm` and returns a JSON payload containing the advertised API version, the OCM endpoint base URL, supported resource types and protocols (including WebDAV), and a set of capability flags.
- Discovery format and capability semantics follow the OCM-API spec. Implementation and docs point to the OCM-API IETF-RFC discovery section as the canonical definition.

Spec: [cs3org/OCM-API IETF-RFC (discovery)](https://github.com/cs3org/OCM-API/blob/615192eeff00bcd479364dfa9c1f91641ac7b505/IETF-RFC.md?plain=1#ocm-api-discovery)

Deliverable:

- [MahdiBaghbani/opencloudmesh-go@v1.0.0](https://github.com/MahdiBaghbani/opencloudmesh-go/tree/v1.0.0)

### Internal testing (unit, integration, and Playwright E2E)

Beyond the OCM Test Suite conformance coverage, the `opencloudmesh-go` repo includes its own test suite at multiple levels. The suite runs locally for quick feedback.

- Unit tests run with `go test -race` across the codebase.
- Integration tests under `tests/integration`, also with race detector.
- Playwright E2E under `tests/e2e`. These tests start server instances with bounded lifecycles, exercise both API and UI flows, and produce debugging artifacts:
  - trace captured on first retry
  - screenshots on failure
  - video recording enabled for each test run

A Playwright smoke test asserts that `/.well-known/ocm` returns valid JSON and the health endpoint responds.

### TLS: certificate loading and ACME auto TLS

The deliverable supports TLS in two ways: loading existing certificate material (controlled environments) and automatic provisioning via ACME (when automated cert management is preferred).

For controlled deployments, TLS uses pre-provisioned certs (in the image or injected at runtime). Works fully offline; HTTPS and discovery behavior can still be validated.

For automated provisioning, the server includes an ACME implementation that supports HTTP-01 challenges. In ACME mode, the server serves challenge responses under `/.well-known/acme-challenge/<token>` on an HTTP listener and then serves the main application over HTTPS using the issued certificate. Works with standard ACME providers; defaults to Let's Encrypt staging and production.

The repo includes integration tests that run ACME end-to-end against a local Pebble server, so the issuance path can be verified without public DNS or external rate limits. Tests perform real issuance and validation, verify certificate files are written to disk, and confirm the service comes up on HTTPS. Pebble keeps production behavior compatible with Let's Encrypt.

Code refs:

- ACME implementation: [MahdiBaghbani/opencloudmesh-go@v1.0.0 (ACME manager)](https://github.com/MahdiBaghbani/opencloudmesh-go/blob/v1.0.0/internal/platform/http/tls/acme.go)
- ACME issuance integration test (Pebble): [MahdiBaghbani/opencloudmesh-go@v1.0.0 (Pebble e2e test)](https://github.com/MahdiBaghbani/opencloudmesh-go/blob/v1.0.0/tests/integration/acme_pebble_test.go)

### Docker deployment

- Added a GitHub Actions workflow to build and publish the `opencloudmesh-go` Docker image to GHCR: [MahdiBaghbani/opencloudmesh-go#1](https://github.com/MahdiBaghbani/opencloudmesh-go/commit/b3fd9d95352a21e743ef73210462f580507e14ee).
- Additional container and integration wiring for the new service in the DockyPody containers repository: [MahdiBaghbani/containers#1](https://github.com/MahdiBaghbani/containers/pull/1).
- Work tracking issue: [cs3org/OCM-STA#13](https://github.com/cs3org/OCM-STA/issues/13)

### OCM Test Suite support

- E2E scenario coverage and CI wiring for `opencloudmesh-go` in the OCM Test Suite: [cs3org/ocm-test-suite#215](https://github.com/cs3org/ocm-test-suite/pull/215). Uses the OCM Test Suite harness plus the repo's own Playwright E2E tests.
- Work tracking issue: [cs3org/OCM-STA#14](https://github.com/cs3org/OCM-STA/issues/14)

## Results vs milestone goals

- Rewrite OCM stub in a compiled language:
  - Go implementation at [MahdiBaghbani/opencloudmesh-go@v1.0.0](https://github.com/MahdiBaghbani/opencloudmesh-go/tree/v1.0.0).

- Implement capability discovery endpoint, loading certificates, ACME auto TLS:
  - The discovery endpoint is exposed at `/.well-known/ocm` in the `opencloudmesh-go` deliverable. The discovery payload follows the OCM-API specification and is directly verifiable from the running service.
  - TLS supported; certificate loading options documented for containerized deployment.
  - ACME auto TLS via HTTP-01 at `/.well-known/acme-challenge/<token>`. Integration tests run real issuance against Pebble and check HTTPS startup.

- Implement better configurability:
  - Config and env vars for service identity (`HOST` / `PUBLIC_ORIGIN`), runtime mode (`OCM_GO_MODE`), config file (`CONFIG`), TLS (`TLS_ENABLED`, `TLS_CERT`, `TLS_KEY`, `TLS_CA`). Supports strict protocol and pragmatic interop. All in the `v1.0.0` tree.

- Implement Docker deploy:
  - Docker build/publish workflow [MahdiBaghbani/opencloudmesh-go#1](https://github.com/MahdiBaghbani/opencloudmesh-go/commit/b3fd9d95352a21e743ef73210462f580507e14ee); container wiring [MahdiBaghbani/containers#1](https://github.com/MahdiBaghbani/containers/pull/1).

- Implement full support in the OCM Test Suite:
  - E2E scenarios and CI wiring [cs3org/ocm-test-suite#215](https://github.com/cs3org/ocm-test-suite/pull/215).

## Notes on compatibility and verification

The discovery document advertises `apiVersion` and capabilities. An interoperability mode tweaks advertised values for specific clients (e.g. Nextcloud Server Crawler) while discovery stays consistent with the spec. You can verify against the OCM-API doc and still test real clients.
