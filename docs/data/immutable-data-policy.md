# Immutable Data Policy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §7.2, §14.2 |

## Immutable and Append-Only Data

| Data | Rule | Enforcement |
|---|---|---|
| Original SourceFiles (bytes) | Never modified or overwritten. Replacement = new Source with `replaces_source_id`. | Object keys include source ID and are write-once; bucket versioning or object lock on `originals/` prefix; application never issues PUT to an existing original key |
| `artifact_versions` | Insert only | `BEFORE UPDATE OR DELETE` trigger raises unless `current_user = 'clerk_retention'` |
| `audit_events` | Append-only from application perspective | `clerk_app` has `INSERT` only; trigger blocks UPDATE/DELETE; optional hash chain ([audit-security](../security/audit-security.md)) |
| `usage_events` | Append-only; corrections are compensating events with negative quantity and `corrects_event_id` | Grants + trigger |
| `approval_decisions` | Append-only | Grants + trigger |
| `consent_records` | Append-only; withdrawal is a new record | Grants |
| `module_versions` (published) | Manifest and package immutable after `published`; only `status` may change (deprecate/yank/block) | Trigger comparing `manifest_sha256` |
| `prompt_versions` (≥ candidate) | Template and schemas immutable | Trigger on `content_sha256` |
| `plan_versions` (active), `plan_entitlements` | Immutable once active; changes = new PlanVersion | Trigger |
| `template_versions` (published), `workflow_versions`, `module_configurations`, `brand_kits`, `data_usage_policies` | New version per change | Application + trigger |
| `evidence_links` | Pointer immutable; only `status` changes (`superseded`, `source_unavailable`) | Trigger restricting updatable columns |
| `source_extractions` | Payload immutable; status transitions only; new pipeline version = new row | Trigger |

## Deletion vs Immutability

Immutability protects against *modification*. Lawful deletion (retention, user/org deletion requests) is still possible via the `clerk_retention` role and audited deletion workflows ([deletion](../privacy/deletion.md)). Audit events are deleted only by retention policy, never by user request, and deletions are themselves audited.

## Related Documents

[versioning-strategy](versioning-strategy.md), [soft-delete-policy](soft-delete-policy.md)
