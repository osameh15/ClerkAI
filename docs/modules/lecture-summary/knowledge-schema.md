# Lecture Summary — Knowledge Schema

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |
| Blueprint | BP §3.4, §10 |

## Entity Subtypes

| Subtype | Base | Attributes | Evidence | Review |
|---|---|---|---|---|
| `lecture.topic` | document_section | `title`, `order`, `time_range?`, `slide_range?` | recommended | — |
| `lecture.concept` | concept | `name`, `aliases[]` (incl. English term for Persian concepts), `summary`, `importance ∈ {core, supporting}` | **required** | threshold 0.75 |
| `lecture.definition` | definition | `term`, `text`, `source_kind ∈ {spoken, slide, handout}` | **required** | threshold 0.80 |
| `lecture.example` | other | `text`, `kind ∈ {worked_example, illustration, case}` | **required** | — |
| `lecture.key_point` | other | `text`, `emphasis ∈ {stated_important, repeated, summary}` | **required** | — |
| `lecture.question` | question | `text`, `kind ∈ {asked_in_class, generated_review}`, `answer?`, `answer_evidence_required` | recommended (generated questions require evidence for answers) | — |
| `lecture.formula` | other | `latex?`, `text`, `variables[]` | **required** | threshold 0.90 |

## Relations

`DEFINES` (definition → concept), `EXEMPLIFIES` (example → concept), `PREREQUISITE_OF` (concept → concept), `PART_OF` (concept/key_point → topic), `RELATED_TO` (concept ↔ concept), `TESTS` (question → concept).

## Grounding

`generated_review` questions and flashcards are derived items; their answers must cite evidence; if not, they are dropped in source-grounded mode.
