# Retention Model (Data Implementation)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (mechanics); Provisional (defaults) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + Tech Lead |
| Blueprint | BP §15.2 |

## Purpose

How retention is implemented in storage. Retention **categories and defaults** are canonical in [privacy/data-retention](../privacy/data-retention.md).

## Mechanics

1. Every retainable row/object is mapped to a **retention category** (`raw_media`, `raw_documents`, `derived_content`, `ai_raw_payloads`, `knowledge`, `artifacts`, `audit`, `usage`, `operational`).
2. Effective policy = organization override (`retention_policies`) → plan default (PlanVersion) → platform default. Resolved at sweep time, never hard-coded.
3. `RetentionSweepWorkflow` runs daily per organization:
   - selects expired items by category using indexed timestamps;
   - deletes objects first (object storage), then database rows, in dependency order;
   - marks dependent evidence `source_unavailable`;
   - writes an audit event summarising counts per category (no content).
4. **Legal hold** (`retention_policies.legal_hold` or per-resource hold flag) suspends deletion.
5. Object storage lifecycle rules act as a safety net (for example abort incomplete multipart uploads after 7 days, expire `ai-raw/` after max 30 days) — never as the primary mechanism for tenant-configurable categories.
6. Backups: deleted data ages out of backups within the backup retention window; documented in [backup-data-model](backup-data-model.md).

## Related Documents

[data-retention](../privacy/data-retention.md), [deletion](../privacy/deletion.md), [object-storage-layout](object-storage-layout.md)
