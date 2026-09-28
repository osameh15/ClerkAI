# Indexing Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | App. E "Key Index / Constraint Checklist" |

## Blueprint Checklist (App. E) — Mandatory

| # | Requirement | Realisation |
|---|---|---|
| 1 | `organizations.slug` and `modules.key` unique; `module_versions(module_id, version)` unique | `uq_organizations__slug`, `uq_modules__key`, `uq_module_versions__module_id__version` |
| 2 | One active membership per (organization, user) | Partial unique `WHERE status IN ('invited','active')` |
| 3 | All foreign keys explicit; restrictive, intentional delete behaviour | [schema-conventions](schema-conventions.md) |
| 4 | `(organization_id, created_at)` and `(organization_id, status)` on Job/Artifact/Source | `ix_jobs__org__created_at`, `ix_jobs__org__status`, same for `artifacts`, `sources` |
| 5 | Partial indexes for active module installations/subscriptions | `WHERE status = 'enabled'`, `WHERE status IN ('trialing','active','past_due')` |
| 6 | GIN/special indexes on JSONB only with proven query need | Requires query evidence in PR |
| 7 | pgvector indexing only after measuring dataset size | [vector-storage](vector-storage.md) |
| 8 | Audit/usage partitioning only when volume justifies; append-only from day one | [partitioning-strategy](partitioning-strategy.md) |

## Additional Baseline Indexes

| Table | Index | Query served |
|---|---|---|
| `sources` | `(organization_id, sha256)` | Tenant dedup |
| `sources` | `(organization_id, project_id, created_at DESC) WHERE deleted_at IS NULL` | Inbox by project |
| `job_sources` | `(organization_id, source_id)` | "Where is this source used" |
| `processing_steps` | `(run_id, step_key)` | Progress, SSE replay |
| `ai_executions` | `(organization_id, created_at)`, `(processing_run_id)` | Cost dashboards, provenance |
| `knowledge_entities` | `(organization_id, job_id, entity_type)` | Artifact generation, review |
| `evidence_links` | `(organization_id, target_type, target_id)`, `(organization_id, source_id)` | Evidence viewer, source deletion impact |
| `review_items` | `(organization_id, job_id, status)`, `(organization_id, assigned_to, status)` | Review queue |
| `artifact_versions` | `(artifact_id, version_no DESC)` | Version history |
| `action_items` | `(organization_id, status, due_date)`, `(organization_id, owner_membership_id, status)` | Actions list, overdue sweep |
| `usage_events` | `(organization_id, meter, occurred_at)` | Aggregation |
| `audit_events` | `(organization_id, occurred_at DESC)`, `(organization_id, resource_type, resource_id)` | Audit queries |
| `outbox_events` | `(id) WHERE published_at IS NULL` | Dispatcher |
| `idempotency_keys` | unique key + `(expires_at)` | Lookup, cleanup |

## Rules

- `organization_id` leads every composite index on tenant tables (RLS predicate + tenant queries).
- Every new query on a table with > 100k expected rows must be checked with `EXPLAIN` in review for hot paths.
- Unused indexes are reviewed quarterly (`pg_stat_user_indexes`).
