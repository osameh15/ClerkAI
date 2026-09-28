# Default Roles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Access domain |
| Blueprint | BP §13, App. C |

## System Roles

| Role | Scope (BP §13) | Intended for |
|---|---|---|
| `owner` | Organization ownership; billing, plan, deletion, ownership transfer | Founder/account holder. ≥ 1 per org. |
| `admin` | Settings, members, modules, roles | IT/operations admins |
| `manager` | Job/Artifact management and approvals within scope | Project managers, team leads |
| `member` | Create and edit within scope | Engineers, staff, lecturers |
| `viewer` | Read-only | Stakeholders |
| `guest` | Limited, temporary or resource-scoped access | External consultants, clients (resource grants) |

System roles are immutable; custom roles can be derived from them ([custom-roles](custom-roles.md)). The full permission mapping is the [permission-matrix](permission-matrix.md).

## Special Rules

- `owner` implicitly holds every platform permission, including future ones.
- `admin` holds `billing.view` and receives `billing.manage` only when delegated by an Owner ("Billing: Admin — Delegated", BP App. C). Delegation is a role assignment of the system permission set `billing-delegate`.
- `manager` approval rights depend on workflow policy ("Approve Artifact: Manager — Per Policy", BP App. C).
- `guest` has no organization-scope permissions; access flows only through `resource_grants` with expiry.
- Personal workspaces have a single `owner`.
