# Data Retention

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (defaults Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + Product |
| Blueprint | BP §15.2 ("Retention is configurable, not hard-coded"; suggested initial defaults) |

## Retention Categories (canonical)

| Category | Covers | Suggested initial default (BP §15.2) | Org configurable | Min/max |
|---|---|---|---|---|
| `raw_media` | Raw audio/video originals + renditions | 90 days for self-service | ✓ (`retention.custom`) | 1 day – indefinite |
| `raw_documents` | Source images/documents originals + renditions | Until user/organization deletion or retention policy | ✓ | — |
| `ai_raw_payloads` | Stored AI request/response payloads | 30 days or less; Enterprise can disable storage | ✓ (shorter/disabled) | 0 – 30 days |
| `derived_content` | Extractions, chunks, embeddings | Aligned with Job/Artifact lifetime | ✓ | ≥ raw only if evidence needs it |
| `knowledge` | Normalized Knowledge + evidence | Aligned with Artifact/Job lifetime | ✓ | — |
| `artifacts` | Artifacts, versions, exports (exports re-renderable; export files 30 d) | Until organization deletion/retention policy | ✓ | — |
| `audit` | Audit events | Security events ≥ 365 days for Business; longer for Enterprise | ✓ (longer only for security events) | floor per plan |
| `usage` | Usage events/aggregates | Billing/legal requirement | ✗ | legal |
| `operational` | Sessions, idempotency keys, notifications, webhook deliveries, outbox | Short (7–90 days) | ✗ | — |

## Rules

- Deletion is **asynchronous and audited**; backup lifecycle stays consistent with policy (deleted data ages out of backups within the backup window) (BP §15.2).
- Deleting raw sources does not delete Knowledge/Artifacts; evidence becomes `source_unavailable` ([evidence-model](../knowledge/evidence-model.md)).
- Legal hold suspends deletion.
- Per-plan defaults and limits: [OD-037](../governance/open-decisions.md#od-037).

Implementation mechanics: [retention-model](../data/retention-model.md).
