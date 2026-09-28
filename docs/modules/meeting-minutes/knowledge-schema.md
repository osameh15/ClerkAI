# Meeting Minutes — Knowledge Schema

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |
| Blueprint | BP §3.1, §10 |

## Entity Subtypes

| Subtype | Base | Attributes | Evidence | Review |
|---|---|---|---|---|
| `meeting.participant` | person | `name`, `membership_id?`, `role_text?`, `organization?`, `speaker_labels[]`, `attended` | recommended | Speaker mapping ambiguity → review (`ambiguous_speaker`) |
| `meeting.agenda_item` | document_section | `number?`, `title`, `order` | recommended (agenda source) | — |
| `meeting.decision` | decision | `text`, `status: decided`, `decision_type?` (approval, rejection, assignment, policy), `conditions?`, `vote?` | **required** | threshold 0.80, blocks finalisation |
| `meeting.proposal` | decision | `text`, `status: proposed`, `proposed_by?` | **required** | — |
| `meeting.discussion_point` | event | `summary`, `agenda_item?` | recommended | — |
| `meeting.unresolved_topic` | question | `text`, `next_step?`, `target_meeting?` | **required** | — |
| `meeting.action` | action | `title`, `description?`, `priority?` | **required**; attribute evidence for `owner`, `deadline` | threshold 0.85, blocks finalisation; missing owner/deadline → warning review item |
| `meeting.deadline` | deadline | `date`, `original_text`, `calendar`, `is_relative` | **required** | date parse failures → review |
| `meeting.risk` | risk | `description`, `impact?`, `mitigation?` | **required** | — |
| `meeting.open_question` | question | `text`, `addressed_to?` | recommended | — |

## Relations

| Relation | From → To | Cardinality |
|---|---|---|
| `RESPONSIBLE_FOR` | participant → action | one participant → many actions (an action may also have external owner text) |
| `DUE_AT` | action → deadline | one-to-one |
| `DECIDED_BY` | decision → participant | many-to-many |
| `RAISED_BY` | risk / open_question / proposal → participant | many-to-many |
| `RELATES_TO_AGENDA` | decision / action / discussion → agenda_item | many-to-many |
| `RESULTS_IN` | decision → action | one-to-many |

## Example (from BP §10)

```text
KnowledgeEntity: meeting.action  title = "Submit revised electrical drawings"  confidence = 0.94
  ├── RESPONSIBLE_FOR ← meeting.participant: Reza Ahmadi
  ├── DUE_AT → meeting.deadline: 1405-07-12 (original "۱۲ مهر", calendar jalali)
  └── Evidence → audio 00:42:18–00:42:31 (S2), notes page 2 bbox #3
```

## Validation Rules (`knowledge.validate`)

- Action without owner → warning review item (`missing owner`), not blocking unless config `requireActionOwner=true`.
- Deadline in the past relative to meeting date → review item (`conflicting_sources` or `schema_gap`).
- Decision without any supporting evidence → dropped or review (never kept silently).
