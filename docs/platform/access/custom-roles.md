# Custom Roles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready ("Limited" definition Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Access domain |
| Blueprint | BP §13, App. C (Custom Roles: Personal —, Team Limited, Business ✓, Enterprise ✓), EPIC-02 |

## Behaviour

- Created via `POST /v1/roles` with a key, name and permission list, optionally `based_on` a system role.
- Entitlement `roles.custom` ∈ `none | limited | full`. `limited` semantics are [OD-030](../../governance/open-decisions.md#od-030); proposed default: at most 3 custom roles, derived from system roles, no module-specific permission removal.
- Validation: unknown permissions rejected (`ACCESS_PERMISSION_UNKNOWN`); a creator cannot include permissions they do not hold; `organization.delete` and `organization.ownership.transfer` cannot be granted to custom roles.
- Changes take effect immediately (cache invalidation) and are audited with before/after permission diff.
- Deleting a role in use → `409 ACCESS_ROLE_IN_USE`.
- Downgrade of plan below `roles.custom` keeps existing custom roles functional but read-only (no silent privilege loss that would lock users out); admins are prompted to migrate.
