# Audit Event Catalog

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (initial) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Audit domain |
| Blueprint | BP §1.1, §14.2, §15.3, §16, App. A |

Action names follow `<domain>.<entity>.<verb>` (past tense). "Sec" marks security-relevant events with extended retention floor.

## Identity and Access

| Action | Sec | Resource | Notes |
|---|---|---|---|
| `identity.session.created` | ✓ | session | method, mfa_level |
| `identity.signin.failed` | ✓ | user/email hash | reason |
| `identity.session.revoked` | ✓ | session | by whom |
| `identity.mfa.enrolled` / `.removed` | ✓ | user | factor type |
| `identity.identity.linked` / `.unlinked` | ✓ | user | provider |
| `organization.member.invited` / `.joined` / `.deactivated` / `.removed` | ✓ | membership | roles |
| `organization.ownership.transferred` | ✓ | organization | from/to |
| `organization.settings.updated` | ✓ | organization | diff |
| `organization.deletion.requested` / `.cancelled` / `.completed` | ✓ | organization | |
| `access.role.created` / `.updated` / `.deleted` | ✓ | role | permission diff |
| `access.assignment.changed` | ✓ | membership | role/scope diff |
| `access.denied` | ✓ | any | permission/entitlement, sampled for high-volume reads |

## Modules

| Action | Sec | Notes |
|---|---|---|
| `module.installed` / `.upgraded` / `.enabled` / `.disabled` / `.suspended` / `.uninstalled` | ✓ | versions, reason |
| `module.configuration.changed` | ✓ | config version + redacted diff |
| `module.assignment.changed` | | |

## Sources, Jobs, AI

| Action | Sec | Notes |
|---|---|---|
| `source.created` | | kind, size, sha256 prefix |
| `source.quarantined` | ✓ | verdict |
| `source.deleted` | | by policy or user |
| `job.created` / `.submitted` / `.cancelled` / `.retried` | | module version pinned |
| `job.completed` / `.failed` | | failure code |
| `ai.generation.performed` | | actor `ai`, on_behalf_of user; job, prompt versions, profiles, models (IDs only) |
| `knowledge.item.confirmed` / `.modified` / `.rejected` | | review item id |

## Artifacts, Templates, Workflow

| Action | Sec | Notes |
|---|---|---|
| `artifact.generated` | | actor `ai`, version |
| `artifact.edited` | | actor `user`, version, change summary |
| `artifact.regenerated` | | actor `system` |
| `artifact.submitted` | | workflow |
| `approval.decided` | ✓ | decision, version |
| `artifact.finalized` | ✓ | document number |
| `artifact.exported` | | format, template version |
| `artifact.shared` / `.share.revoked` | ✓ | share type |
| `artifact.deleted` / `.restored` | | |
| `template.published` / `brand_kit.updated` / `numbering.changed` | | |
| `workflow.definition.changed` | ✓ | |

## Commerce and Integrations

| Action | Sec | Notes |
|---|---|---|
| `subscription.changed` | ✓ | plan versions, seats |
| `entitlement.override.created` / `.expired` | ✓ | source, reason |
| `billing.budget.changed` | ✓ | |
| `api_key.created` / `.revoked` | ✓ | prefix, scopes |
| `webhook.created` / `.updated` / `.deleted` / `.secret.rotated` | ✓ | URL host only |

## Privacy and Governance

| Action | Sec | Notes |
|---|---|---|
| `privacy.retention_policy.changed` | ✓ | category, durations |
| `privacy.data_usage_policy.changed` | ✓ | training/evaluation flags |
| `privacy.consent.recorded` / `.withdrawn` | ✓ | consent type, policy version |
| `privacy.export.requested` / `.completed` | ✓ | |
| `privacy.deletion.requested` / `.completed` | ✓ | counts by category |
| `privacy.retention.swept` | | counts by category |
| `platform.admin.access` | ✓ | break-glass, ticket, scope, duration |
| `audit.exported` | ✓ | range |
