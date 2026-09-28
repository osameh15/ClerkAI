# ADR-004: Public API = REST + OpenAPI

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §6, §8, §28.1, App. B |

## Context

The Nuxt PWA, a future public API, webhooks consumers and third-party modules all need one stable, documented contract. The frontend must not know internal processing stages (BP §8).

## Decision

- **REST** over HTTPS with JSON for CRUD and commands; commands on resources as `POST /v1/<resource>/{id}/<verb>` (for example `/v1/jobs/{id}/submit`).
- **OpenAPI 3.1** document at [`api/openapi/openapi.yaml`](../../api/openapi/openapi.yaml) is the contract. It is written first and validated in CI; server types and the Web SDK are generated from it ([OD-025](../open-decisions.md#od-025)).
- URL-major versioning (`/v1`). Platform API version is independent of Module versions.
- Long-running operations are asynchronous: commands return `202 Accepted` with the resource; progress is read via SSE ([ADR-021](ADR-021-sse-progress.md)) or polling.

## Consequences

- Breaking changes require `/v2` or additive evolution per [api-versioning](../../api/api-versioning.md).
- GraphQL/gRPC are not exposed publicly in v1.

## Alternatives Considered

GraphQL (flexible reads, harder authorization and caching per object); gRPC (poor browser fit).

## Related

[api-principles](../../api/api-principles.md), [errors](../../api/errors.md), [idempotency](../../api/idempotency.md)
