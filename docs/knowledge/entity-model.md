# Knowledge Entity Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain |
| Blueprint | BP §10, §10.1 |

## Base Entity Types (platform)

BP §10.1 baseline plus two generic helpers. Modules extend with namespaced subtypes.

| Base type | Typical attributes | Examples of module subtypes |
|---|---|---|
| `person` | `name`, `role_text`, `membership_id?`, `aliases[]` | `meeting.participant` |
| `organization` | `name`, `kind` | `site.responsible_party` |
| `date` | `date`, `original_text`, `calendar` | `handwriting.date` |
| `deadline` | `date`, `original_text`, `calendar`, `is_relative` | `meeting.deadline`, `site.deadline` |
| `location` | `label`, `building/floor/zone`, `gps?` | `site.location` |
| `decision` | `text`, `status ∈ {decided, proposed}` | `meeting.decision`, `meeting.proposal` |
| `action` | `title`, `description` | `meeting.action`, `site.corrective_action`, `handwriting.task` |
| `issue` | `description`, `category`, `severity` | `site.issue` |
| `risk` | `description`, `likelihood?`, `impact?` | `meeting.risk` |
| `question` | `text`, `status ∈ {open, answered}` | `meeting.open_question`, `lecture.question` |
| `observation` | `description`, `category`, `condition` | `site.observation` |
| `concept` | `name`, `summary` | `lecture.concept` |
| `definition` | `term`, `text` | `lecture.definition` |
| `amount` | `value`, `unit`, `currency?`, `original_text` | `handwriting.amount` |
| `object` | `name`, `description` | equipment, material |
| `event` | `title`, `time?` | `site.visit`, `meeting.discussion_point` |
| `document_section` | `heading?`, `text`, `order` | `handwriting.text_block`, `lecture.topic` |
| `other` | module-defined | `lecture.example`, `handwriting.uncertain_span` |

## Stored Fields

`entity_type` (base), `subtype` (module key-namespaced), `schema_ref` (attribute schema), `attributes` (validated JSON), `display_text` (short label for UI/search), `confidence`, `confidence_band`, `review_state`, `origin`, `revision`, `supersedes_id`, `processing_run_id`, `ai_execution_id`.

## Canonicalization

Within a job, fusion merges duplicates into one canonical entity:
- `person`: normalised names (Persian normalisation, honorific stripping, transliteration variants), match to organization members by name/email when confident; otherwise stays unresolved text + Review Item if the role requires a member (for example action owner).
- `deadline`/`date`: resolve to ISO date with original text and calendar; relative expressions resolved against the source's `captured_at` with evidence ([OD-026](../governance/open-decisions.md#od-026)).
- `location`: normalise labels against project location vocabulary (module configuration) when provided.
- Merged duplicates are kept as `supersedes` chains; evidence links are unioned.

## Validation

Attributes validated against the module attribute schema; failure → the item is not persisted and the step records a schema error (feeds repair/fallback, [schema-validation](../ai/schema-validation.md)).
