# API Client (Web SDK)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (generator Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §8 ("The Clerk Web SDK sits on top of a versioned REST API"), §29.3 (Web SDK generation / thin wrapper) |

## Layers

1. **Generated client** (`packages/web-sdk`) from [openapi.yaml](../api/openapi/openapi.yaml) — types + request functions ([OD-025](../governance/open-decisions.md#od-025)).
2. **Wrapper** (`services/api`) adds:
   - credentials (`include`) and `X-CSRF-Token`;
   - `Clerk-Organization-Id` from org store;
   - `Idempotency-Key` generation for commands (stable per user action; persisted for offline queue);
   - `If-Match` from cached ETags;
   - error mapping to typed `ClerkApiError { code, status, retryable, meta, requestId }`;
   - retries with backoff for `retryable` errors/429 (respecting `Retry-After`), never for 4xx validation;
   - SSE helper (`useJobStream`) with reconnect + polling fallback.
3. **Composables** for screens.

## Contract Testing

CI verifies the generated client compiles against the current OpenAPI; breaking changes caught by the API diff check.
