# Site Report — Knowledge Schema

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |
| Blueprint | BP §3.2, §10 |

## Entity Subtypes

| Subtype | Base | Attributes | Evidence | Review |
|---|---|---|---|---|
| `site.visit` | event | `date`, `inspection_type`, `weather?` | none | — |
| `site.location` | location | `label`, `building?`, `floor?`, `zone?`, `grid?`, `gps?` | recommended | Unmatched to vocabulary → review (warning) |
| `site.observation` | observation | `description`, `category`, `condition ∈ {conforming, non_conforming, info}`, `capture_group_id?`, `media_refs[]` | **required** (`description`, `location`) | threshold 0.75, blocking |
| `site.issue` | issue | `description`, `category`, `severity ∈ {low, medium, high, critical}`, `status ∈ {open, resolved}`, `first_observed_at`, `reinspection_count` | **required** (`severity`, `category`) | `critical` always reviewed; blocking |
| `site.corrective_action` | action | `title`, `description?`, `due_days_default?` | recommended; attribute evidence for `responsible_party`, `deadline` | missing responsible party → warning |
| `site.responsible_party` | organization | `name`, `kind ∈ {contractor, subcontractor, internal_team, supplier, other}`, `membership_id?` | recommended | — |
| `site.deadline` | deadline | `date`, `original_text?`, `derived_from ∈ {stated, severity_default}` | recommended | Defaults from severity labelled as such |

"Overdue" is not stored on the issue; it is derived from linked corrective Action Items ([action-items](../../platform/actions/action-items.md)).

## Relations

| Relation | From → To |
|---|---|
| `OBSERVED_AT` | observation → location |
| `INDICATES` | observation → issue |
| `CORRECTED_BY` | issue → corrective_action |
| `RESPONSIBLE_FOR` | responsible_party → corrective_action |
| `DUE_AT` | corrective_action → deadline |
| `REINSPECTS` | observation (current visit) → issue (previous visit, same project) — cross-job, project-scoped |

## Media Linkage

Observation `media_refs` point to source files (photos/video frames) and the voice note chunk that describes them; evidence links provide the precise pointers (bbox on photo, time range in voice note).
