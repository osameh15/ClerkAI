# Permission Namespaces

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Access domain |
| Blueprint | BP §13.1 |

## Format

`<namespace>(.<sub>)*.<action>` — lowercase, dot-separated, kebab-case segments. Regex in [common.schema.json](../../api/schemas/common.schema.json) (`permissionKey`). Keys are stable forever; deprecated keys remain in the catalogue with `deprecated_at`.

- **Platform namespaces:** `organization`, `roles`, `billing`, `modules`, `projects`, `sources`, `jobs`, `artifacts`, `templates`, `workflows`, `actions`, `audit`, `integrations`, `privacy`, `security`.
- **Module namespaces:** the module key (`business.meeting-minutes`, `construction.site-report`, …). A module may only register permissions under its own key (validated at manifest validation).

## Platform Permission Catalogue (v1)

Blueprint BP §13.1 examples are marked (BP).

| Key | Description |
|---|---|
| `organization.settings.view` | View organization settings |
| `organization.settings.manage` | Change organization settings (name, locale, timezone) |
| `organization.members.view` | View members |
| `organization.members.invite` (BP) | Invite members |
| `organization.members.remove` | Remove/deactivate members |
| `organization.teams.manage` | Create/edit teams and team membership |
| `organization.delete` | Delete organization (Owner only) |
| `organization.ownership.transfer` | Transfer ownership (Owner only) |
| `roles.view` | View roles and permission catalogue |
| `roles.manage` (BP) | Create/edit custom roles |
| `roles.assign` | Assign roles to memberships |
| `billing.view` (BP) | View plan, usage, invoices |
| `billing.manage` | Change plan, seats, payment methods, budgets |
| `modules.view` | View Store and installations |
| `modules.install` (BP) | Install, enable, disable, upgrade, uninstall modules |
| `modules.configure` (BP) | Change module configuration |
| `modules.assign` | Assign modules to teams/projects |
| `projects.view` | View projects in scope |
| `projects.create` | Create projects |
| `projects.manage` | Edit/archive projects, manage project members |
| `sources.view` | View sources in scope |
| `sources.create` | Upload/capture sources |
| `sources.delete` | Delete sources (policy permitting) |
| `jobs.view` | View jobs in scope |
| `jobs.create` (BP) | Create jobs (with module `.create`) |
| `jobs.cancel` | Cancel jobs (own; any in scope via `jobs.manage`) |
| `jobs.retry` | Retry failed jobs |
| `jobs.manage` | Cancel/retry any job in scope |
| `jobs.review` | Resolve review items (with module `.edit`) |
| `artifacts.view` | View artifacts in scope |
| `artifacts.edit` (BP) | Edit own artifacts |
| `artifacts.edit-any` | Edit any artifact in scope |
| `artifacts.export` (BP) | Export artifacts |
| `artifacts.approve` (BP) | Approve artifacts (with module `.approve`, per policy) |
| `artifacts.share` | Share artifacts |
| `artifacts.delete` | Delete artifacts |
| `templates.view` | View templates and brand kit |
| `templates.manage` | Manage templates, brand kit, numbering |
| `workflows.manage` | Customise approval workflows |
| `actions.view` | View action items |
| `actions.create` | Create action items |
| `actions.manage` | Edit any action item in scope |
| `audit.view` | Query audit log |
| `audit.export` | Export audit log |
| `integrations.manage` | API keys and webhooks |
| `privacy.manage` | Retention, AI/data usage policy, training consent |
| `privacy.requests` | Initiate organization data export/deletion |
| `security.manage` | Session revocation for others, MFA enforcement, SSO settings |

## Module Permission Conventions

| Suffix | Meaning |
|---|---|
| `.create` | Create jobs of this module |
| `.view` | View module jobs/artifacts in scope |
| `.edit` | Edit module artifacts, resolve review items |
| `.approve` | Approve module artifacts |
| other | Module-specific (for example `construction.site-report.resolve-items`, `business.meeting-minutes.view-transcript`) |

Examples (BP §13.1 and brief): `business.meeting-minutes.create`, `business.meeting-minutes.edit`, `business.meeting-minutes.approve`, `construction.site-report.create`, `construction.site-report.edit`, `construction.site-report.approve`.
