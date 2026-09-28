# API Principles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8, App. B |

## Purpose

Design rules for every Clerk HTTP endpoint. The contract is [openapi.yaml](openapi/openapi.yaml) ([ADR-004](../governance/adr/ADR-004-rest-openapi.md)).

## Principles

| # | Principle | Rule |
|---|---|---|
| API-01 | Contract first | Endpoints are added to OpenAPI and reviewed before implementation. CI validates the document and runs contract tests against the server. |
| API-02 | Resource-oriented | Nouns in plural (`/jobs`, `/artifacts`). Commands that are not CRUD are sub-resources with verbs: `POST /jobs/{id}/submit`. |
| API-03 | Tenant context explicit | Session principals send `Clerk-Organization-Id`; API keys are bound to one org. Nested org paths (`/organizations/{id}/members`) must match the header when both present. |
| API-04 | Async by default for work | Anything that may exceed ~2 s returns `202` with a resource to observe (Job, Export, ModuleOperation). |
| API-05 | Frontend-agnostic of modules | No module-specific endpoints. Module differences flow through schemas (job input, configuration, artifact content) and manifests ([ADR-009](../governance/adr/ADR-009-schema-driven-module-ui.md)). |
| API-06 | Idempotent commands | See [idempotency](idempotency.md). |
| API-07 | Optimistic concurrency | Mutable resources return `ETag`; edits of artifacts require `If-Match` ([concurrency-strategy](../architecture/concurrency-strategy.md)). |
| API-08 | Stable errors | Single error shape and namespaced codes ([errors](errors.md), [error-catalog](error-catalog.md)). |
| API-09 | No existence oracles | Resources in other tenants or outside the caller's scope return `404`, not `403`. `403` is used when the resource is visible but the action is not permitted. |
| API-10 | Cursor pagination | [pagination-filtering-sorting](pagination-filtering-sorting.md). |
| API-11 | JSON conventions | `snake_case` fields; RFC 3339 UTC timestamps; IDs as UUID strings; enums lowercase `snake_case`; `null` rather than omitted for known-but-empty nullable fields in responses; unknown request fields rejected (`additionalProperties: false`) to catch client bugs. |
| API-12 | Localisation | `Accept-Language` selects message language for errors and localised strings; data is never translated implicitly. |
| API-13 | Customer-facing units | Usage endpoints expose AI Credits/minutes/pages/storage, never tokens ([BDR-002](../governance/bdr/BDR-002-cost-aware-pricing.md)). |
| API-14 | Security headers | `Cache-Control: no-store` on tenant data; CORS allowlist; HSTS at the edge ([api-security](../security/api-security.md)). |

## Standard Headers

| Header | Direction | Meaning |
|---|---|---|
| `Clerk-Organization-Id` | Request | Active organization |
| `Idempotency-Key` | Request | Command dedup |
| `If-Match` | Request | Optimistic concurrency |
| `X-Correlation-Id` | Request (first-party only) / Response | End-to-end operation id |
| `X-Request-Id` | Response | Per-request id (also in error body) |
| `ETag` | Response | Resource version |
| `Retry-After` | Response | With 429/503 |
| `X-CSRF-Token` | Request | Cookie-authenticated unsafe methods |
| `RateLimit-Limit`, `RateLimit-Remaining`, `RateLimit-Reset` | Response | [rate-limiting](rate-limiting.md) |

## Endpoint Status Markers

OpenAPI operations carry `x-clerk-status` (`baseline`, `refinement`, `provisional`, `deferred`). `baseline` endpoints come from BP App. B; `refinement` endpoints complete Blueprint flows (for example upload abort, export status, additional artifact generation) without contradicting it.

## Related Documents

[api-versioning](api-versioning.md), [authentication](authentication.md), [authorization](authorization.md), [examples](examples.md)
