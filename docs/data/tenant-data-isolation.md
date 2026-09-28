# Tenant Data Isolation (Database)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + Security |
| Blueprint | BP §7.2, §14.1, §14.2, App. D, App. F |

## Purpose

Database-level realisation of [tenancy-architecture](../architecture/tenancy-architecture.md) and [ADR-013](../governance/adr/ADR-013-multi-tenant-isolation.md).

## Database Roles

| Role | Used by | Privileges | RLS |
|---|---|---|---|
| `clerk_migrator` | Migration job only | DDL, owner of schemas | Bypass (owner) |
| `clerk_app` | API, workers | DML on tenant and platform tables per schema grants; `INSERT` only on `audit_events`, `usage_events`, `approval_decisions`, `consent_records` | **Subject to RLS** (`NOBYPASSRLS`, not table owner) |
| `clerk_dispatcher` | Outbox dispatcher | `SELECT/UPDATE/DELETE` on outbox, inbox tables | N/A (no RLS on outbox) |
| `clerk_retention` | Retention/deletion workflows | Delete privileges on tenant tables incl. append-only ones, via `SECURITY DEFINER` procedures only | Sets `app.org_id` per org |
| `clerk_platform_admin` | Audited admin tooling | Read on selected tables across tenants | Separate policy `rls_*__platform_admin` gated by `app.admin_session` + audit |
| `clerk_readonly_analytics` | BI (future) | Views with aggregated/non-content data | Views only |

## Session Variables

Set by the Tx manager at the start of every transaction (`SET LOCAL`, so they cannot leak across pooled connections):

| Variable | Value |
|---|---|
| `app.org_id` | Tenant organization UUID |
| `app.actor_id` | User/API key/system identifier (for audit triggers) |
| `app.actor_type` | `user`, `api_key`, `system`, `ai`, `platform_admin` |
| `app.request_id` | For trigger-based diagnostics |

Connection poolers must run in transaction mode compatible with `SET LOCAL` (PgBouncer transaction pooling is compatible).

## Policy Template

```sql
ALTER TABLE job.jobs ENABLE ROW LEVEL SECURITY;
ALTER TABLE job.jobs FORCE ROW LEVEL SECURITY;

CREATE POLICY rls_jobs__tenant ON job.jobs
  USING (organization_id = current_setting('app.org_id', true)::uuid)
  WITH CHECK (organization_id = current_setting('app.org_id', true)::uuid);
```

For `T?` tables (platform rows with null org + tenant rows, for example `roles`, `templates`):

```sql
CREATE POLICY rls_roles__tenant ON access.roles
  USING (organization_id IS NULL OR organization_id = current_setting('app.org_id', true)::uuid)
  WITH CHECK (organization_id = current_setting('app.org_id', true)::uuid);
```

(Tenants can read platform rows but never write them.)

## Tables Under RLS

All tables marked `RLS = Y` in the [entity-catalog](entity-catalog.md). At minimum (sensitive tables, BP §7.2): organizations, memberships, invitations, teams, membership_roles, resource_grants, projects, subscriptions, organization_entitlements, usage_events, module_installations, module_configurations, upload_sessions, sources, source_files, source_extractions, source_chunks, chunk_embeddings, jobs, job_sources, processing_runs, processing_steps, ai_executions, knowledge_entities, knowledge_relations, evidence_links, review_items, artifacts, artifact_versions, artifact_exports, brand_kits, templates, template_versions, numbering_sequences, workflow_instances, approval_requests, approval_decisions, action_items, comments, shares, notifications, api_keys, webhook_endpoints, webhook_deliveries, audit_events, retention_policies, data_usage_policies, consent_records, training_examples, idempotency_keys.

## Composite Foreign Keys (cross-tenant reference protection)

Parents expose `UNIQUE (organization_id, id)`. Children reference both:

```sql
ALTER TABLE job.job_sources
  ADD CONSTRAINT fk_job_sources__source
  FOREIGN KEY (organization_id, source_id) REFERENCES source.sources (organization_id, id);
```

This prevents a row in org A from referencing a Source in org B even if application code passes a foreign ID.

## Pre-Tenant Lookups

Before an organization is selected (sign-in → workspace list) the API calls `SECURITY DEFINER` functions that take the authenticated `user_id` and return only that user's memberships and organization display data. These functions are owned by `clerk_migrator`, have fixed `search_path`, and are covered by tests.

## Background Workers

Workers set `app.org_id` from the workflow/event tenant before each transaction. A worker transaction without `app.org_id` sees no tenant rows (fails safe). Cross-organization maintenance (retention sweeps) iterates organizations, one transaction per organization.

## Testing (required in CI; BP App. F "Tenant-isolation integration tests run in CI")

1. For every RLS table: a query without `app.org_id` returns 0 rows; inserts fail.
2. With org A context, rows of org B are invisible and not updatable.
3. Composite-FK test: inserting a child referencing another org's parent fails.
4. BOLA API tests: every `{id}` endpoint with another tenant's ID returns `404` (not `403`, to avoid existence oracles).
5. Test that `clerk_app` has `NOBYPASSRLS` and does not own tables.

## Related Documents

[security/tenant-isolation](../security/tenant-isolation.md), [database-testing](../testing/database-testing.md)
