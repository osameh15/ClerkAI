# Site Report — Permissions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Access |
| Blueprint | BP §13.1 (`construction.site-report.create`, `.approve`) |

| Permission | Default roles | Notes |
|---|---|---|
| `construction.site-report.create` | owner, admin, manager, member | Project scope applies |
| `construction.site-report.view` | owner, admin, manager, member, viewer, guest (grants) | Clients/consultants as guests via project grants |
| `construction.site-report.edit` | owner, admin, manager, member | Own visits for members; any in project for managers |
| `construction.site-report.approve` | owner, admin, manager | + workflow stage eligibility (project roles) |
| `construction.site-report.resolve-items` | owner, admin, manager, member | Resolve corrective actions with evidence |

Project roles `project_manager` and `consultant` are referenced by the default approval workflow; organizations map members to these project roles in project membership.
