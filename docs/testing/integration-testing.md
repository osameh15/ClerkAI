# Integration Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §19 (Integration: Object storage, Temporal, Redis, AI mocks/provider sandbox) |

## Environment

Testcontainers (or CI service containers): PostgreSQL 16 + pgvector, Redis, MinIO, Temporal dev server, mock AI provider, ClamAV (EICAR test), Mailpit.

## Suites

| Suite | Assertions |
|---|---|
| Upload flow | Presign → PUT to MinIO → complete → Source + outbox + audit; checksum mismatch; expired session; multipart resume |
| Ingestion | Pipelines produce extractions/chunks for fixtures of each kind; cache reuse; tenant dedup |
| Outbox/events | Events dispatched in order per subject; consumer dedup under duplicate delivery; dead-letter after N failures |
| Redis fallbacks | Kill Redis → API continues (rate limit local, cache bypass) |
| AI Gateway | Routing, retry, repair, fallback with mock provider scenarios (429, 5xx, malformed JSON, schema violation, content rejection); one AIExecution per attempt; usage events once |
| Authorization + RLS | Cross-tenant fixtures at repository and API level |
| Exports | Render fixture artifacts via renderer container; outputs stored under tenant prefix |
| Webhooks | Delivery signing, retries, SSRF blocklist |
