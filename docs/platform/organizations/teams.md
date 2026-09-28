# Teams

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Organizations domain |
| Blueprint | BP §7, §13 (Team scope), App. A |

## Purpose

Teams group memberships for **scoping** (role assignments at team scope, module assignments, resource visibility) and for approver rules. Teams are not tenants and do not own data exclusively; resources carry an optional `team_id` when created in a team context.

## Rules

- Team names unique per organization (case-insensitive).
- `team_role ∈ {member, lead}`; `lead` can be referenced by approval workflow rules (`approvers: { rule: team_lead }`).
- Deleting a team (soft) removes team-scoped role assignments and module assignments; resources keep `team_id` for history but visibility falls back to organization-scope rules.
- Events: `organization.team.changed`.
