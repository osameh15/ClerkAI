# ADR-012: Critical Commands = Idempotent

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §8.3, §28.1 |

## Context

Mobile clients retry on flaky networks; Temporal retries activities; event delivery is at-least-once. Duplicate submits, installs or exports would double-charge usage and duplicate artifacts.

## Decision

- Sensitive commands (at minimum: upload create/complete, job submit/cancel/retry, module install/enable/disable/upgrade, export, approval decision, review resolve, API-key creation, subscription changes) **require** an `Idempotency-Key` header.
- Keys are scoped by `(organization_id, principal, method+route, key)`; the server stores a request fingerprint and response for 24 hours (configurable). Replays return the stored response; a different body with the same key returns `422 IDEMPOTENCY_KEY_REUSED`; a concurrent duplicate still in progress returns `409 IDEMPOTENCY_REQUEST_IN_PROGRESS`.
- Temporal activities use deterministic activity IDs and upsert semantics. Event consumers use inbox deduplication by `event_id`.
- AI fallback creates a separate AIExecution per attempt (BP §8.3); usage is recorded once per AIExecution.

## Related

[idempotency-strategy](../../architecture/idempotency-strategy.md), [api/idempotency](../../api/idempotency.md)
