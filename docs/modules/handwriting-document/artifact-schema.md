# Handwriting → Document — Artifact Schemas

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.3, §12 |

| Type | Schema ref | Generation | Body |
|---|---|---|---|
| `clean_document` (primary) | `general.handwriting-document/clean-document@1.0.0` | automatic, mapping | `title?`, `blocks[] { kind (heading/paragraph/list/table), content: richText | table, source_page }`, faithful to original wording (spelling normalised only if config allows) |
| `formal_letter` | `…/formal-letter@1.0.0` | on demand, AI-composed + bindings | `recipient { name, title, organization }`, `subject`, `salutation`, `body` (paragraphs), `closing`, `signatory`, `references[]`, `attachments_note` — must not introduce facts absent from Knowledge; stylistic formalisation allowed |
| `meeting_minutes` | `…/meeting-minutes@1.0.0` (compatible subset of `business.meeting-minutes/minutes`) | on demand, AI | decisions, actions, participants from notes |
| `task_list` | `…/task-list@1.0.0` | on demand, mapping | `items[] { title, owner_text, deadline: dateValue, source_page }` |

## Letter Rules

- Letter wording may rephrase for formality but every factual statement (names, dates, amounts, commitments) must bind to Knowledge with evidence; unbound facts are flagged `unsupported`.
- Letterhead, numbering (`L-{yyyy}-{seq:5}`) and signature via Brand Kit and template.
