# ADR-013: Tenant Isolation = Application Guard + PostgreSQL RLS

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership, Security |
| Blueprint | BP §1.1, §7.2, §14.1, §28.1, App. D |

## Context

Cross-tenant data leakage is the highest-severity risk for a B2B document platform. A single missing `WHERE organization_id = ?` must not be sufficient to leak data.

## Decision

Two independent layers:

1. **Application guard (primary):** every request/workflow/activity carries an immutable `TenantContext{organization_id, principal, …}`. Repositories require it as an argument; tenant-owned queries always filter by it; object-level authorization is checked on every endpoint.
2. **PostgreSQL RLS (second line of defence):** sensitive tenant tables have `ENABLE` + `FORCE ROW LEVEL SECURITY` policies comparing `organization_id` with `current_setting('app.org_id')`, set with `SET LOCAL` inside each transaction. The application DB role has no `BYPASSRLS`. Platform-admin access uses a separate, audited path and role.

Tenant context also scopes: object-storage key prefixes, Redis keys, event envelopes, Temporal workflow inputs, vector retrieval, logs/traces attributes (where safe).

## Consequences

- Shared tables (plans, module catalog, AI models) are explicitly classified as platform-scoped and have no RLS; everything else defaults to tenant-scoped.
- CI must include cross-tenant (BOLA) tests; acceptance: no cross-tenant access in authorization/RLS tests (BP App. D).

## Related

[tenancy-architecture](../../architecture/tenancy-architecture.md), [tenant-data-isolation](../../data/tenant-data-isolation.md), [security/tenant-isolation](../../security/tenant-isolation.md)
