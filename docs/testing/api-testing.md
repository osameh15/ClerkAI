# API Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead + QA |
| Blueprint | BP §19, App. B |

## Coverage per Endpoint

| Aspect | Tests |
|---|---|
| Happy path | Status, schema-valid body, headers (`ETag`, `X-Request-Id`) |
| Validation | Unknown fields rejected, required fields, enum values → `422`/`400` with details |
| Authentication | Missing/expired credentials → 401 |
| Authorization matrix | Each system role (and API key scope) × operation → expected allow/deny ([permission-matrix](../platform/access/permission-matrix.md)) |
| Entitlements | Missing entitlement → 402 with `required_entitlement` |
| BOLA | Foreign-tenant and out-of-scope IDs → 404 |
| Idempotency | Replay returns same response; different body → 422; concurrent → 409 |
| Concurrency | Stale `If-Match` → 412; missing → 428 where required |
| Pagination | Cursor stability, tampered cursor → 400 |
| Rate limiting | Exceed → 429 with `Retry-After` |
| State transitions | Illegal transitions → 409 `*_INVALID_TRANSITION` |
| SSE | Stream frames, reconnect with `Last-Event-ID`, termination at terminal state |
