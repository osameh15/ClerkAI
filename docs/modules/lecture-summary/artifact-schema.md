# Lecture Summary — Artifact Schemas

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.4, §12 |

| Type | Schema ref | Generation | Body |
|---|---|---|---|
| `lecture_summary` (primary) | `education.lecture-summary/lecture-summary@1.0.0` | automatic, hybrid | `header {course, title, date, lecturer}`, `overview` (paragraphs), `topics[] { title, key_points[], concepts[] refs, time_range, slide_refs }`, `definitions[] { term, text }`, `examples[]`, `questions_raised[]`, `concept_map { nodes[], edges[] }` |
| `study_notes` | `…/study-notes@1.0.0` | on demand, AI | Structured notes per topic with headings, bullet explanations, formulas, cross-references; every paragraph cites |
| `flashcards` | `…/flashcards@1.0.0` | on demand, AI | `cards[] { front, back, concept_ref, difficulty, evidence_ref }` |
| `question_set` | `…/question-set@1.0.0` | on demand, AI | `questions[] { type ∈ {recall, conceptual, application}, text, answer, concept_refs[], evidence_ref }` |

## Rendering

Flashcards print template (cut lines, two-sided layout) for PDF; interactive flashcards viewer via hybrid extension `artifact.flashcards-viewer`; concept map via `artifact.concept-map` (web only; PDF renders as a relationship table).
