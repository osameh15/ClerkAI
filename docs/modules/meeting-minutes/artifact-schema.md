# Meeting Minutes — Artifact Schemas

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Module Platform |
| Blueprint | BP §3.1, §12 |

All bodies use platform `$defs` from [artifact.schema.json](../../api/schemas/artifact.schema.json).

## `minutes` (primary, `business.meeting-minutes/minutes@1.0.0`)

| Path | Type | Source | Bound |
|---|---|---|---|
| `meeting.title` | string | job input | — |
| `meeting.date` | dateValue | job input / capture | — |
| `meeting.location` | string | job input | — |
| `meeting.chair` | personRef | job input / knowledge | ✓ |
| `meeting.participants[]` | personRef | participants | ✓ |
| `meeting.absent[]` | personRef | job input | — |
| `agenda[]` | `{ number, title, summary: richText, decisions[], actions[] refs }` | agenda items + fusion | ✓ |
| `decisions[]` | `{ text: richText, decided_by[], conditions?, agenda_ref? }` | `meeting.decision` | ✓ |
| `proposals[]` | `{ text, proposed_by? }` | `meeting.proposal` | ✓ |
| `unresolved[]` | `{ text, next_step? }` | `meeting.unresolved_topic` | ✓ |
| `actions[]` | `{ title, owner: personRef, deadline: dateValue, status }` | `meeting.action` + relations | ✓ |
| `risks[]` | `{ description, mitigation? }` | `meeting.risk` | ✓ |
| `open_questions[]` | `{ text }` | `meeting.open_question` | ✓ |
| `summary` | paragraphs | AI (`summarize`), cites knowledge | partial |
| `next_meeting` | `{ date?, location? }` | knowledge/job input | — |
| `signatures[]` | `{ role, name }` | approval/brand kit | — |

Metadata: `document_number` placeholder filled at finalisation.

## `action_list` (`…/action-list@1.0.0`)

`items[] { title, owner, deadline, status, source_ref (decision/agenda), evidence_summary }` — deterministic mapping.

## `executive_summary` (on demand, `…/executive-summary@1.0.0`)

`headline`, `key_decisions[]` (≤ 5), `critical_actions[]`, `risks[]`, `summary` (paragraphs) — hybrid.

## `follow_up` (on demand, `…/follow-up@1.0.0`)

`subject`, `greeting`, `body` (paragraphs), `action_table`, `closing` — AI-composed from Knowledge; Clerk does not send it.

## `transcript` (secondary, on demand, `…/transcript@1.0.0`)

`segments[] { start_ms, end_ms, speaker (participant ref or label), text }` — generated from extraction + attribution; visibility requires `business.meeting-minutes.view-transcript`.
