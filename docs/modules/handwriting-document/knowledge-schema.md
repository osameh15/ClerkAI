# Handwriting → Document — Knowledge Schema

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |
| Blueprint | BP §3.3, §10 |

## Entity Subtypes

| Subtype | Base | Attributes | Evidence | Review |
|---|---|---|---|---|
| `handwriting.text_block` | document_section | `text`, `order`, `block_kind ∈ {paragraph, heading, list_item, table_cell, margin_note}`, `page` | **required** (bbox) | threshold 0.85, blocks finalisation |
| `handwriting.uncertain_span` | other | `text_candidate?`, `alternatives[]`, `legibility ∈ {uncertain, illegible}`, `parent_block_id` | **required** | always reviewed, blocks finalisation |
| `handwriting.heading` | document_section | `text`, `level` | recommended | — |
| `handwriting.list_item` | document_section | `text`, `list_id`, `order` | recommended | — |
| `handwriting.person` | person | `name` | **required** | — |
| `handwriting.date` | date | `date?`, `original_text`, `calendar` | **required** | threshold 0.90, blocking |
| `handwriting.amount` | amount | `value?`, `unit?`, `currency?`, `original_text` | **required** | threshold 0.90, blocking |
| `handwriting.task` | action | `title`, `owner_text?`, `deadline?` | **required**, attribute evidence owner/deadline | — |

## Relations

`CONTAINS` (text_block → uncertain_span/person/date/amount), `FOLLOWS` (reading order), `RESPONSIBLE_FOR` (person → task).

## No Silent Invention (BP App. D)

`uncertain_span` with `legibility = illegible` must be resolved by the user as: typed value (confirmed), explicit `[illegible]` marker, or deletion. Artifact generation renders unresolved spans as highlighted placeholders and finalisation is blocked until resolved.

## Reuse for Other Artifacts

Meeting-minutes output maps `handwriting.*` entities to meeting semantics via an AI step (`compose_artifact` with `artifactType: meeting_minutes`), creating `meeting.*`-compatible Knowledge only if `business.meeting-minutes` schemas are available (optional dependency); otherwise uses the module's bundled `meeting_minutes` schema.
