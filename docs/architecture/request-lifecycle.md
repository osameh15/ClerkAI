# Request Lifecycle

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8, §13, §14.2, §17 |

## Purpose

Exact order of middleware and checks for every HTTP request to `clerk-api`.

## Pipeline

```mermaid
flowchart TB
  A[1. Receive request<br/>assign request_id, extract/assign correlation_id] --> B[2. OTel span start<br/>structured logger with ids]
  B --> C[3. Limits: body size, timeout, IP rate limit]
  C --> D[4. Authentication<br/>session cookie or API key]
  D --> E[5. CSRF check<br/>cookie flows, unsafe methods]
  E --> F[6. Tenant resolution<br/>membership or key binding → TenantContext]
  F --> G[7. Principal/org rate limits]
  G --> H[8. Idempotency check<br/>for commands]
  H --> I[9. Decode + validate against OpenAPI schema]
  I --> J[10. Use case]
  J --> K[11. Authorization inside use case<br/>entitlement → permission → resource policy]
  K --> L[12. Transaction: SET LOCAL app.org_id,<br/>state change, outbox, audit, usage]
  L --> M[13. Post-commit: start/signal Temporal]
  M --> N[14. Store idempotent response]
  N --> O[15. Map result/error → HTTP<br/>X-Request-Id, ETag]
```

## Rules

- `request_id`: generated per request (UUIDv7), returned as `X-Request-Id`, included in every error body.
- `correlation_id`: accepted from `X-Correlation-Id` only for authenticated first-party clients; otherwise generated. Propagated to Temporal and events.
- Authorization happens in the **use case**, not only in middleware, so object-level checks see the loaded resource (BOLA protection).
- Post-commit Temporal start failure does not roll back the business change; a reconciler detects `queued` jobs without a running workflow and starts them ([failure-model](failure-model.md)).
- Response time budget: non-processing endpoints p95 < 500 ms (provisional SLO, BP §17.1).

## Related Documents

[api-principles](../api/api-principles.md), [authorization-model](../platform/access/authorization-model.md), [idempotency](../api/idempotency.md), [errors](../api/errors.md)
