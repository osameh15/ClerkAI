# Partitioning Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Provisional (activation thresholds) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) / SRE |
| Blueprint | App. E ("Audit/usage partitioning only when real volume justifies it; the contract is append-only from day one") |

## Principle

No partitioning in v1 unless measured volume justifies it. Tables are designed so partitioning can be introduced later without changing application contracts: time column present, append-only, primary keys compatible with partition keys.

## Candidates

| Table | Partition key (future) | Activation trigger (provisional) | Retention operation |
|---|---|---|---|
| `audit_events` | `occurred_at` monthly range | > 50M rows or retention deletes > 1% table/day | Drop partitions older than max org retention; per-org shorter retention via delete |
| `usage_events` | `occurred_at` monthly range | > 50M rows | Archive to object storage (Parquet) after billing close + legal retention |
| `ai_executions` | `created_at` monthly | > 20M rows | Detach + archive |
| `outbox_events` | `occurred_at` daily | Dispatcher cleanup cost | Drop old partitions (7 days) |
| `processing_steps` | none initially | — | Retained with runs |
| `source_chunks`, `chunk_embeddings` | `organization_id` hash (only if vector index size requires) | Measured | With source deletion |
| `webhook_deliveries`, `notifications` | `created_at` monthly | Volume | Drop after 30/90 days |

## Constraints to Preserve Now

- Unique constraints on high-volume tables include or avoid the future partition key (for example `event_id` uniqueness enforced with `(event_id)` now; on partitioning it becomes `(event_id, occurred_at)` with a dedup check in the writer).
- Queries on these tables always include a time range.
