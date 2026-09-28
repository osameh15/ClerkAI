# Backup

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §14.4, §17.1 (RPO ≤ 15 min), §18 |

What and how: [backup-data-model](../data/backup-data-model.md).

## Operational Requirements

- PostgreSQL continuous WAL archiving + daily base backups to separate, encrypted, access-restricted storage (different account/project where possible).
- Backup success monitored; failure alerts ([alerting](alerting.md)).
- Object storage: versioning for originals; cross-region replication when required.
- Backup access limited to SRE break-glass; all access audited.
- Retention 35 days (provisional); legal/Enterprise requirements may extend with documented privacy impact.
