# PDR-008: Personal Workspace Is a One-Person Organization

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Product, Technical Leadership |
| Blueprint | BP §2 ("A Personal Workspace can be implemented as a one-person organization") |

## Decision

A Personal Workspace is an `organizations` row with `kind = 'personal'` and exactly one Owner membership. Billing, entitlements, data ownership and tenant isolation behave exactly as for team organizations. A personal workspace may later be converted to `kind = 'team'` (upgrade) without data migration.

## Consequences

No separate "user-owned data" code path. A user can belong to one personal workspace and many team organizations and switches context explicitly.

## Related

[personal-workspaces](../../platform/organizations/personal-workspaces.md)
