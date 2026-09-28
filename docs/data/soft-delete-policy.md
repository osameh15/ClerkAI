# Soft-Delete Policy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + Privacy |
| Blueprint | BP §7.2, §15.2, §15.3 |

## Principles

1. Soft delete (`deleted_at`) is a **user-experience grace period** and referential-safety mechanism, not a retention mechanism. Soft-deleted data is still personal data and must be hard-deleted by the retention/deletion workflow.
2. Status-based lifecycles (`removed`, `uninstalled`, `archived`, `revoked`) are preferred over `deleted_at` for business states with meaning.
3. Default queries exclude soft-deleted rows (repository convention + partial indexes `WHERE deleted_at IS NULL`).

## Per-Entity Behaviour

| Entity | User delete action | Grace period | Hard delete by |
|---|---|---|---|
| Source | Soft delete; detached from draft Jobs; evidence for final artifacts → `source_unavailable` after hard delete | 30 days (configurable) | Retention workflow (objects first, then metadata tombstone) |
| Job | Soft delete if not in processing; cancels active run | 30 days | Retention workflow with Knowledge/Artifacts per policy |
| Artifact | Soft delete; blocked if `final` and organization policy requires retention | 30 days | Retention workflow |
| Project | Archive (status) then soft delete when empty | — | Manual/retention |
| Team | Soft delete; memberships unaffected | — | Retention |
| Comments | Soft delete (content replaced by tombstone) | 0 days content, row kept for thread integrity | Retention |
| Webhook endpoints | Soft delete; deliveries stop immediately | 30 days | Retention |
| Membership | Status `removed`; records authored remain (BP §15.3) | — | Never deleted with org alive; user personal data handled by user deletion |
| Module installation | Status `uninstalled` ([module-disable-uninstall](../modules/module-disable-uninstall.md)) | — | Not deleted; history retained |
| Organization | Status `pending_deletion` (grace period) → `deleted` → full purge | 30 days | Organization deletion workflow |
| User | Status `deleted`; PII scrubbed; memberships removed; authored content stays attributed to "Deleted user" | 30 days | User deletion workflow |

## Restore

Restore is allowed within the grace period by users with the delete permission; it clears `deleted_at` and is audited.
