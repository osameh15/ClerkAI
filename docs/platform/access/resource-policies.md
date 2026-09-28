# Resource Policies

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Access domain + owning contexts |
| Blueprint | BP §13 ("RBAC is only the first layer; Project/Team/Data scope is also evaluated"), §14.2 |

## Resource Attributes

Every protected resource exposes a `ResourceRef`: `type`, `id`, `organization_id`, `owner_membership_id`, `team_id?`, `project_id?`, `state`, `classification?`.

## Policy Rules (v1)

| Rule | Description |
|---|---|
| RP-1 Tenant | `resource.organization_id == tc.organization_id` (always; also enforced by RLS). |
| RP-2 Scope | Permission must be granted at organization scope, or at the resource's team/project scope. |
| RP-3 Ownership | "Own" permissions (`artifacts.edit`, `jobs.cancel`) require `owner_membership_id == caller` unless the caller has the `-any`/`manage` variant. |
| RP-4 Module assignment | If the module installation is assigned to specific teams/projects, only members of those scopes can create jobs for it. |
| RP-5 Grants | `resource_grants` / shares add access (view/comment/edit) to specific resources for guests, teams or members, with optional expiry. |
| RP-6 State | Resource state constrains actions: final artifacts are not editable; disabled modules block job creation; quarantined sources block attachment. |
| RP-7 Approval policy | Approver must be listed/qualified in the workflow version stage (role/project role/user), and cannot approve own submission if policy `separation_of_duties=true`. |
| RP-8 Sensitive fields | Module permissions can gate fields (for example transcript visibility requires `business.meeting-minutes.view-transcript`) — returned as `obligations`. |

## Visibility vs Action

List endpoints filter by visibility (RP-1..RP-5). Detail endpoints return `404` for invisible resources (no existence disclosure) and `403` for visible resources where the action is denied.

## Implementation

Policies live in the access context as pure functions over `ResourceRef` + effective assignments; owning contexts provide `ResourceRef` loaders. Repository list queries include scope predicates generated from the caller's assignments (no N+1 post-filtering beyond pagination windows).
