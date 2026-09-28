# Restore

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §14.4 (Private Beta: backup restore test), §18 ("Backup restore is tested periodically") |

## Restore Procedures

| Scenario | Procedure |
|---|---|
| Point-in-time recovery (logical corruption) | Restore to new instance at chosen time → verify → switch or selectively copy data |
| Full database loss | Restore latest base + WAL → run reconciliation (objects vs metadata, Temporal vs runs) → re-apply deletion requests completed after backup time |
| Single-tenant data recovery | Restore to isolated instance → export tenant rows (under RLS with tenant context) → import with audit (Enterprise support) |
| Object deletion by mistake | Restore from object versioning within retention |

## Restore Test (monthly)

Restore production backup to isolated restore environment (access-controlled) → automated integrity checks (row counts, checksums on sample, application boot, smoke tests) → report recorded; failures are SEV3 incidents.
