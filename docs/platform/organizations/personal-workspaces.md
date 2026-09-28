# Personal Workspaces

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Organizations domain |
| Blueprint | BP §2, [PDR-008](../../governance/pdr/PDR-008-personal-workspace-as-organization.md) |

## Rules

- `organizations.kind = 'personal'`, exactly one member (Owner); invitations disabled (`ORG_SEAT_LIMIT_REACHED` via `seats.max = 1`).
- Same tenancy, entitlements, billing and audit as team organizations; typical plan: Personal.
- A user may own at most one personal workspace (proposed; prevents free-tier farming).
- **Conversion to team:** allowed by upgrading to a team plan; `kind` changes to `team`; no data migration.
- Lecture Summary B2C/B2Edu users typically operate in a personal workspace.
