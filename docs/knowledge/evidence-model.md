# Evidence Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain |
| Blueprint | BP §1.1 (Evidence-first), §10, §30.1, App. D (Evidence ≥ 90% openable), [ADR-011](../governance/adr/ADR-011-evidence-first.md) |

## Purpose

**Canonical** evidence specification. Machine contract: [evidence.schema.json](../api/schemas/evidence.schema.json). Evidence is structured data, not UI decoration.

## Pointer Types

| Pointer | Fields | Used for |
|---|---|---|
| `time_range` | `media` (audio/video), `start_ms`, `end_ms`, `speaker_label?` | Utterances in recordings |
| `page` | `page`, `slide?`, `sheet?`, `cell_range?` | Document pages, slides, spreadsheet ranges |
| `bbox` | `page?`, `frame_ms?`, `x`, `y`, `width`, `height` (normalised 0–1), `region_label?` | Image regions, handwriting lines, video frame regions |
| `text_span` | `start`, `end` (code points in extraction text), `page?` | Text documents, notes |
| `chunk` | `chunk_id` | Coarse evidence when finer location unavailable |

Example (brief): *Deadline: 12 Mehr* — Evidence: `audio.mp3 00:21:12–00:21:18` (`time_range` 1272000–1278000 ms) and `notes.jpg bounding-box #3` (`bbox` with `region_label: "#3"`).

## Link Targets

`target_type ∈ {entity, relation, artifact_field}`, `target_id`, optional `target_path` (JSON Pointer to an attribute, for example `/attributes/deadline`). Attribute-level evidence lets an action's owner and deadline cite different moments.

## Support

`supports` (default), `contradicts` (conflicting source), `context` (surrounding information). Contradicting evidence triggers `conflicting_sources` review.

## Lifecycle

- Created by AI steps (from validated chunk citations mapped to precise locators), by humans (attaching/selecting evidence during review), or by system rules (for example metadata).
- Immutable pointer; status transitions: `active` → `superseded` (entity revision replaced evidence) or `source_unavailable` (source deleted by retention/user — metadata kept: source title, pointer, quote if retained).
- Evidence references `extraction_id` so pointers stay valid when newer pipeline versions exist.

## Validation Rules

1. Referenced chunk/extraction must belong to a Source attached to the same Job (or, for cross-job modules, a declared related Job in the same project).
2. Time ranges within media duration; pages within page count; boxes within [0,1].
3. AI-proposed evidence must cite chunk handles actually present in the prompt ([hallucination-controls](../ai/hallucination-controls.md)).
4. `quote` must be a substring (normalised) of the cited chunk text when present; mismatch → evidence dropped and item flagged.

## Evidence Coverage Metric

`key facts with ≥1 active, resolvable evidence / key facts` where key facts = entity types with `evidence: required` ([product-kpis](../product/product-kpis.md), [evaluation-metrics](../ai/evaluation/evaluation-metrics.md)).

## Access

Opening evidence requires read access to the Source; the viewer fetches a short-lived presigned URL for the rendition and seeks to the pointer ([evidence-ui](../frontend/evidence-ui.md), [evidence-display](../artifacts/evidence-display.md)).
