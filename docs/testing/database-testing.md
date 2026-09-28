# Database Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §19 (Repository: PostgreSQL queries + RLS), §18 (DB migration test), App. F (tenant-isolation tests in CI) |

| Suite | Assertions |
|---|---|
| Migrations | Apply all migrations to empty DB; upgrade from previous release snapshot; down/rollback scripts for expand steps; destructive-change lint |
| Schema vs catalog | Every table in [entity-catalog](../data/entity-catalog.md) exists with owner schema, RLS flag and required columns (`organization_id`, timestamps) |
| RLS | Per RLS table: no `app.org_id` → 0 rows and insert fails; org A cannot see/modify org B; `WITH CHECK` blocks wrong org inserts |
| Roles | `clerk_app` NOBYPASSRLS, not owner; INSERT-only on append-only tables; triggers block UPDATE/DELETE on immutable tables |
| Composite FKs | Cross-tenant child references rejected |
| Constraints | Unique active membership, one active subscription, one non-uninstalled installation, version uniqueness |
| Repositories | Each repository method tested against real PostgreSQL with tenant contexts |
| Concurrency | Job transition locks; numbering sequence gap-free under concurrent finalisation; outbox `SKIP LOCKED` |
| Performance smoke | EXPLAIN checks for hot queries use intended indexes (fixtures with representative sizes) |
