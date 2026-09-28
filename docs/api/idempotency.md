# Idempotency (HTTP Contract)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8.3 |

## Contract

- Header: `Idempotency-Key: <8–128 chars>` (UUID recommended). Required on operations marked `x-clerk-idempotency: required` in [openapi.yaml](openapi/openapi.yaml); optional (but honoured) on other `POST`s.
- Scope: `(organization, principal, method, route template, key)`.
- Retention: 24 hours (configurable).
- Fingerprint: SHA-256 of the canonicalised JSON body (+ relevant headers such as `If-Match`).

| Situation | Response |
|---|---|
| First request | Executed; response stored |
| Replay, same fingerprint, completed | Stored status and body returned, header `Idempotent-Replayed: true` |
| Replay while first is still executing | `409 IDEMPOTENCY_REQUEST_IN_PROGRESS`, `Retry-After: 1` |
| Same key, different fingerprint | `422 IDEMPOTENCY_KEY_REUSED` |
| Missing key on required operation | `400 IDEMPOTENCY_KEY_REQUIRED` |

- Stored responses include error responses for `4xx` validation failures, but **not** `5xx`/`429` (those may be retried with the same key and re-execute).
- Offline PWA queue generates the key at capture time so retries across app restarts remain idempotent ([upload-queue](../pwa/upload-queue.md)).

System-wide design: [idempotency-strategy](../architecture/idempotency-strategy.md).
