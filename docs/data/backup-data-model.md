# Backup Data Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (targets Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps / SRE |
| Blueprint | BP §14.4, §15.2, §17.1, §18 |

## What Is Backed Up

| Store | Method | Frequency | Retention (provisional) | Target |
|---|---|---|---|---|
| PostgreSQL (business) | Continuous WAL archiving + daily base backup (PITR) | Continuous | 35 days | RPO ≤ 15 min (BP §17.1) |
| Temporal persistence DB | Daily snapshot + WAL where supported | Daily | 7 days | Workflows resumable; business state re-derivable from PostgreSQL |
| Object storage `sources`, `platform` | Provider durability + versioning; cross-region replication when [OD-028](../governance/open-decisions.md#od-028) requires | Continuous | Version retention 30 days | Durability |
| Object storage `exports`, `ai-payloads` | Not backed up (re-renderable / ephemeral) | — | — | — |
| Redis | Not backed up | — | — | Ephemeral |
| Secrets | Secret manager native backup | — | — | — |
| Configuration / IaC | Git | — | — | — |

## Consistency

- PostgreSQL and object storage are not transactionally consistent. On restore, a **reconciliation job** compares `source_files` metadata with objects: missing object → mark file `missing` and source `partially_processed`; orphan object → schedule deletion.
- Temporal restore older than PostgreSQL: the reconciler restarts/re-signals workflows from PostgreSQL state (PostgreSQL wins).

## Deletion and Backups

Deleted tenant data persists in backups up to the backup retention (35 days). This is disclosed in privacy documentation. Restores of backups older than a completed deletion request must re-apply deletion requests from `data_deletion_requests` before the environment is returned to service.

## Restore Testing

"Backup restore is tested periodically, not merely backup existence" (BP §18). Monthly restore test to an isolated environment with integrity checks; required for Private Beta gate (BP §14.4). See [restore](../operations/restore.md).
