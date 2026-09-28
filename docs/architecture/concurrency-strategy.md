# Concurrency Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8, App. B ("Optimistic-concurrency edit"), §12 ("Transaction-safe sequence") |

## Purpose

How concurrent writers are coordinated: optimistic vs pessimistic control per aggregate, and system-level concurrency limits.

## Aggregate-Level Control

| Aggregate | Strategy | Mechanism |
|---|---|---|
| Artifact edits | Optimistic | `artifacts.row_version`; client sends `If-Match: "<etag>"`; mismatch → `412 ARTIFACT_VERSION_CONFLICT` with current version id |
| Job state transitions | Pessimistic short lock | `SELECT … FOR UPDATE` on job row within transition tx; transitions validated by state machine |
| Knowledge review resolution | Optimistic | `review_items.row_version`; concurrent resolve → `409 REVIEW_ITEM_ALREADY_RESOLVED` |
| Module installation operations | Pessimistic + single operation | Row lock on installation; only one in-flight operation (`operation_id`) per installation |
| Numbering sequences | Pessimistic | `UPDATE numbering_sequences SET next_value = next_value + 1 … RETURNING` in the finalisation tx (gap-free per organization policy) |
| Quota reservation | Atomic counter | Redis `INCRBY` with PostgreSQL reconciliation; hard limits re-checked in PostgreSQL for billing-critical meters |
| Approval decisions | Pessimistic on request row | Decision insert + stage completion evaluation in one tx |
| Organization settings, templates, brand kits | Optimistic | `row_version` + `If-Match` |
| Outbox dispatch | Row claim | `FOR UPDATE SKIP LOCKED` |
| Dispatcher leadership | Advisory lock | `pg_try_advisory_lock` |

## System-Level Concurrency

| Limit | Scope | Default (provisional) | Enforcement |
|---|---|---|---|
| Concurrent processing Jobs | Organization | From entitlement `jobs.concurrent` | Checked at submit; Temporal workflow waits (queue) if exceeded rather than failing |
| Concurrent uploads | Principal | 10 sessions | Upload session creation |
| AI calls | Provider/model | Provider rate limits | Redis token bucket + per-worker semaphore |
| AI budget | Organization | Monthly budget | Gateway pre-check + post-accounting |
| Export renders | Worker | 4 | Temporal worker `MaxConcurrentActivityExecutionSize` |

## ETag Format

`ETag: "v<row_version>"` for mutable resources; ArtifactVersions (immutable) use `"av-<version_id>"`. Weak ETags are not used for concurrency control.

## Related Documents

[api/idempotency](../api/idempotency.md), [artifact-versioning](../artifacts/artifact-versioning.md), [quotas](../billing/quotas.md), [jobs/concurrency](../jobs/concurrency.md)
