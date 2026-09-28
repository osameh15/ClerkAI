# Cancellation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain |
| Blueprint | BP §8.1, App. A (Processing: cancel), App. B |

## Semantics

1. API validates transition and records `cancel_requested_at` + audit in one transaction; responds `202`.
2. For `queued|processing|needs_review`, the API requests Temporal workflow cancellation (post-commit; reconciler retries).
3. The workflow's cancellation handler: stops scheduling new steps; running activities receive cancellation via heartbeat context and stop at safe points; compensation activities run (release quota reservations, abort in-flight exports, mark run `cancelled`).
4. Job → `cancelled` when the workflow acknowledges (or immediately for `draft|ready`).

## What Is Kept

- Sources and their extractions (reusable).
- Knowledge and draft artifact versions produced before cancellation (marked `from_cancelled_run`), visible but not auto-finalised.
- Usage already incurred (billable; provider cost was spent).

## What Is Undone

- Quota reservations not consumed.
- Pending approval instances for artifacts of this job are cancelled.

## Idempotency

Cancelling a cancelled job returns `202` with current state (natural idempotency); `Idempotency-Key` still required.
