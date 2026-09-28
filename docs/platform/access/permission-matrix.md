# Permission Matrix (Baseline)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Access domain |
| Blueprint | App. C "Baseline Role Matrix" |

## Blueprint Capability Matrix (App. C, verbatim semantics)

| Capability | Owner | Admin | Manager | Member | Viewer | Guest |
|---|---|---|---|---|---|---|
| View Module / Job | ✓ | ✓ | ✓ | ✓ | ✓ | Limited |
| Create Job | ✓ | ✓ | ✓ | ✓ | — | Limited |
| Edit Own Artifact | ✓ | ✓ | ✓ | ✓ | — | — |
| Edit Any Artifact in Scope | ✓ | ✓ | ✓ | — | — | — |
| Approve Artifact | ✓ | ✓ | Per Policy | — | — | — |
| Install / Disable Module | ✓ | ✓ | — | — | — | — |
| Manage Roles | ✓ | ✓ | — | — | — | — |
| Billing | ✓ | Delegated | — | — | — | — |
| Audit | ✓ | ✓ | Per Entitlement | — | — | — |
| Delete Organization | ✓ | — | — | — | — | — |

## Permission-Level Matrix (derived)

Legend: ✓ granted at organization scope · S granted within assigned team/project scope · G only through resource grants · D delegated · P per workflow policy · E per entitlement · — not granted.

| Permission | owner | admin | manager | member | viewer | guest |
|---|---|---|---|---|---|---|
| `organization.settings.view` | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| `organization.settings.manage` | ✓ | ✓ | — | — | — | — |
| `organization.members.view` | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| `organization.members.invite` | ✓ | ✓ | — | — | — | — |
| `organization.members.remove` | ✓ | ✓ | — | — | — | — |
| `organization.teams.manage` | ✓ | ✓ | — | — | — | — |
| `organization.delete` | ✓ | — | — | — | — | — |
| `organization.ownership.transfer` | ✓ | — | — | — | — | — |
| `roles.view` | ✓ | ✓ | ✓ | — | — | — |
| `roles.manage` | ✓ | ✓ | — | — | — | — |
| `roles.assign` | ✓ | ✓ | — | — | — | — |
| `billing.view` | ✓ | ✓ | — | — | — | — |
| `billing.manage` | ✓ | D | — | — | — | — |
| `modules.view` | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| `modules.install` | ✓ | ✓ | — | — | — | — |
| `modules.configure` | ✓ | ✓ | — | — | — | — |
| `modules.assign` | ✓ | ✓ | — | — | — | — |
| `projects.view` | ✓ | ✓ | S | S | S | G |
| `projects.create` | ✓ | ✓ | ✓ | — | — | — |
| `projects.manage` | ✓ | ✓ | S | — | — | — |
| `sources.view` | ✓ | ✓ | S | S | S | G |
| `sources.create` | ✓ | ✓ | ✓ | ✓ | — | G |
| `sources.delete` | ✓ | ✓ | S | own | — | — |
| `jobs.view` | ✓ | ✓ | S | S | S | G |
| `jobs.create` | ✓ | ✓ | ✓ | ✓ | — | G |
| `jobs.cancel` / `jobs.retry` | ✓ | ✓ | S | own | — | — |
| `jobs.manage` | ✓ | ✓ | S | — | — | — |
| `jobs.review` | ✓ | ✓ | S | own | — | — |
| `artifacts.view` | ✓ | ✓ | S | S | S | G |
| `artifacts.edit` (own) | ✓ | ✓ | ✓ | ✓ | — | — |
| `artifacts.edit-any` | ✓ | ✓ | S | — | — | — |
| `artifacts.export` | ✓ | ✓ | ✓ | ✓ | — | — |
| `artifacts.approve` | ✓ | ✓ | P | — | — | — |
| `artifacts.share` | ✓ | ✓ | S | own | — | — |
| `artifacts.delete` | ✓ | ✓ | S | — | — | — |
| `templates.view` | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| `templates.manage` | ✓ | ✓ | — | — | — | — |
| `workflows.manage` | ✓ | ✓ | — | — | — | — |
| `actions.view` | ✓ | ✓ | S | S | S | G |
| `actions.create` | ✓ | ✓ | ✓ | ✓ | — | — |
| `actions.manage` | ✓ | ✓ | S | own | — | — |
| `audit.view` | ✓ | ✓ | E | — | — | — |
| `audit.export` | ✓ | ✓ | — | — | — | — |
| `integrations.manage` | ✓ | ✓ | — | — | — | — |
| `privacy.manage` | ✓ | ✓ | — | — | — | — |
| `privacy.requests` | ✓ | — | — | — | — | — |
| `security.manage` | ✓ | ✓ | — | — | — | — |
| `<module>.create` | ✓ | ✓ | ✓ | ✓ | — | G |
| `<module>.view` | ✓ | ✓ | S | S | S | G |
| `<module>.edit` | ✓ | ✓ | ✓ | ✓ | — | — |
| `<module>.approve` | ✓ | ✓ | P | — | — | — |

Notes:
- "own" = permission granted but resource policy RP-3 limits it to resources the member created/owns.
- "Limited" for Guest in BP App. C is realised as G (resource grants with expiry).
- Module rows reflect default manifests; each module's `permissions.md` may add module-specific keys.
- Entitlements are **not** shown here; they are evaluated before permissions ([plan-entitlements](../entitlements/plan-entitlements.md)).
