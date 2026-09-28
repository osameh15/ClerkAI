# Deletion

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + Tech Lead |
| Blueprint | BP §15.2, §15.3 ("Deletion requests run asynchronously with visible status") |

## Deletion Scopes

| Scope | Initiated by | Effect |
|---|---|---|
| Source | User with `sources.delete` | Objects + extractions + chunks + embeddings deleted; evidence → `source_unavailable` |
| Job | User | Job, runs, knowledge, draft artifacts deleted; final artifacts per policy (may block) |
| Artifact | User with `artifacts.delete` | Versions + exports deleted (final artifacts may be protected by policy/legal hold) |
| Module data | Owner | All jobs/artifacts of a module (after uninstall) |
| User | User (self) | Account PII erased; memberships removed; authored org content remains attributed to "Deleted user" |
| Organization | Owner (MFA) | All tenant data purged after grace period |

## Workflow

```mermaid
sequenceDiagram
  participant U as Requester
  participant API as clerk-api
  participant DB as PostgreSQL
  participant W as DeletionWorkflow
  participant S3 as Object storage
  U->>API: delete request (scope)
  API->>DB: data_deletion_requests(pending) + audit privacy.deletion.requested
  API-->>U: 202 + request id (status visible)
  W->>DB: check legal hold / policy blocks
  W->>S3: delete objects (reference-counted) by scope
  W->>DB: delete rows in dependency order per owning context (via facades)
  W->>DB: mark evidence source_unavailable where applicable
  W->>DB: request completed + report (counts per category) + audit privacy.deletion.completed
```

Each owning context implements a `DeleteByScope` facade; events `privacy.deletion.requested` fan out to contexts that must clean derived data (search indexes, caches).

## Guarantees

- Visible status (`pending`, `running`, `completed`, `blocked`, `failed`).
- Idempotent and resumable.
- Backups: data persists until backup expiry (disclosed); restores re-apply completed deletion requests ([backup-data-model](../data/backup-data-model.md)).
- Audit events about the deletion are retained (without deleted content).
