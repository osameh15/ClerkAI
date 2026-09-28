# Audit Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Audit domain + Security |
| Blueprint | BP §1.1 (Audit by default), §7.2, §14.2 ("Immutable audit events…"), §15.2 (Security audit ≥ 365 days Business), App. A (Audit Log), App. C |

## Purpose

Canonical definition of audit events: an append-only, tenant-queryable record of who did what, when, to which resource, with what outcome — distinguishing AI, human and system actors.

## Audit vs Observability

Audit is a business/security record with retention obligations and customer access; observability is operational telemetry ([glossary](../../governance/glossary.md#disambiguation)). Audit is written **in the same database transaction** as the audited change via the `AuditWriter` port — not derived from logs or events — so an audited change cannot commit without its audit record.

## Record Structure (`audit_events`)

| Field | Notes |
|---|---|
| `id` (BIGINT), `event_uuid` | Internal/external identifiers |
| `organization_id` | Null for platform-level events (platform admin actions are recorded both globally and under the affected org) |
| `occurred_at` | Transaction timestamp |
| `actor_type` | `user`, `api_key`, `system`, `ai`, `platform_admin`, `remote_module` |
| `actor_id`, `actor_display` | Display snapshot preserved even if the user is later deleted (tombstoned per privacy rules) |
| `on_behalf_of` | For system/AI actions triggered by a user (for example AI generation for a job submitted by user X) |
| `action` | From the [audit-event-catalog](audit-event-catalog.md) |
| `resource_type`, `resource_id` | Target |
| `outcome` | `success`, `denied`, `failure` |
| `request_id`, `correlation_id` | Link to telemetry |
| `ip_prefix`, `user_agent_hash` | For security events |
| `changes` | Redacted before/after diff for configuration changes (never document content) |
| `metadata` | Action-specific IDs (for example `artifact_version_id`, `prompt_version`, `model_key`) |
| `prev_hash`, `hash` | Optional per-organization hash chain for tamper evidence ([audit-security](../../security/audit-security.md)) |

## Actor Provenance (required distinctions)

| Situation | `actor_type` | `on_behalf_of` |
|---|---|---|
| AI generated a draft | `ai` | submitting user |
| Human edited artifact | `user` | — |
| Human approved | `user` | — |
| System regenerated after review | `system` | reviewing user |
| Retention sweep deleted data | `system` | — |
| Support engineer via break-glass | `platform_admin` | — (plus ticket id in metadata) |

## Guarantees

- Append-only from the application perspective (INSERT-only grant + trigger).
- Retention per organization policy with minimum floors per plan (≥ 365 days for Business security events, longer for Enterprise, BP §15.2).
- Queryable by actor, action, resource, time (`GET /v1/audit`, entitlement `audit.log`); exportable (`audit.export`).
- Audit writes never contain customer content, secrets, or tokens.

## Related Documents

[audit-event-catalog](audit-event-catalog.md), [audit-security](../../security/audit-security.md), [data-retention](../../privacy/data-retention.md)
