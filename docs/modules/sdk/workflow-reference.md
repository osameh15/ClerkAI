# Workflow Reference (Declarative Processing Definitions)

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Provisional ([OD-018](../../governance/open-decisions.md#od-018)) |
| Version | 0.9.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain + Module Platform |
| Blueprint | BP §4 (workflows/ in package), §4.2 (declarative runtime), §8 |

## Format

A processing workflow is a YAML DAG of **platform step types**, interpreted by the generic `JobProcessingWorkflow`.

```yaml
# workflows/processing.yaml (business.meeting-minutes, abridged)
definitionVersion: "1.0"
key: meeting-processing
stages:                         # user-facing stage keys for SSE/timeline
  - { key: preparing_sources, label: { en: Preparing sources, fa: آماده‌سازی منابع } }
  - { key: understanding, label: { en: Understanding the meeting, fa: تحلیل جلسه } }
  - { key: review, label: { en: Review, fa: بررسی } }
  - { key: drafting, label: { en: Drafting minutes, fa: تهیه صورتجلسه } }
steps:
  - key: sources
    type: ingest.require
    stage: preparing_sources
    params:
      slots:
        recording: { kinds: [transcript, diarization, language], required: false }
        agenda: { kinds: [text, layout], required: false }
        notes: { kinds: [text, handwriting_ocr], required: false }
      softDeadline: PT30M
  - key: align
    type: knowledge.fuse
    stage: understanding
    needs: [sources]
    params:
      strategies: [temporal, semantic]
      anchors: { slot: agenda, entityType: meeting.agenda_item }
  - key: extract_minutes
    type: ai.extract
    stage: understanding
    needs: [align]
    params:
      task: extract_minutes              # manifest ai.tasks key
      input: { slots: [recording, agenda, notes], batching: by_agenda_item }
      produces: [meeting.decision, meeting.proposal, meeting.discussion_point, meeting.unresolved_topic, meeting.risk, meeting.open_question]
  - key: extract_actions
    type: ai.extract
    stage: understanding
    needs: [align]
    params:
      task: extract_actions
      input: { slots: [recording, notes], batching: by_time_window }
      produces: [meeting.action, meeting.deadline, meeting.participant]
      relations: [RESPONSIBLE_FOR, DUE_AT, RESULTS_IN]
  - key: resolve
    type: knowledge.resolve
    stage: understanding
    needs: [extract_minutes, extract_actions]
    params: { canonicalize: [person, deadline], matchMembers: true }
  - key: gate
    type: review.gate
    stage: review
    needs: [resolve]
    params: { mode: blocking_only, beforeArtifacts: false }
  - key: minutes
    type: artifact.generate
    stage: drafting
    needs: [resolve]
    params: { artifactType: minutes, mode: hybrid, mapping: artifacts/minutes.mapping.yaml, aiTask: summarize }
  - key: action_list
    type: artifact.generate
    stage: drafting
    needs: [resolve]
    params: { artifactType: action_list, mode: mapping, mapping: artifacts/action-list.mapping.yaml }
  - key: wait_review
    type: review.await
    stage: review
    needs: [gate, minutes, action_list]
    params: { regenerate: [minutes, action_list] }
```

## Step Types (platform, contract v1.0)

| Type | Purpose | Key params |
|---|---|---|
| `ingest.require` | Ensure extractions for slots exist (reuse/cache), wait for them | `slots.{slot}.kinds`, `required`, `softDeadline` |
| `knowledge.fuse` | Align sources, create alignment relations | `strategies`, `anchors`, `window` |
| `ai.extract` | Run an AI task producing Knowledge | `task`, `input` (slots, retrieval, batching), `produces`, `relations` |
| `ai.generate` | AI-composed content for artifacts (fields) | `task`, `artifactType`, `fields` |
| `knowledge.resolve` | Canonicalise, merge duplicates, match members, detect conflicts | `canonicalize`, `matchMembers` |
| `knowledge.validate` | Declarative validation rules (for example action must have owner) | `rules[]` (JSON Logic subset) |
| `knowledge.link_related` | Cross-job linking within declared scope (project) | `scope`, `fromTypes`, `toTypes`, `task` (matching) |
| `review.gate` | Evaluate thresholds, create review items; optionally block before artifacts | `mode` (`blocking_only`, `always`, `none`), `beforeArtifacts` |
| `review.await` | Durable wait until blocking items resolved; regenerate listed artifacts | `regenerate[]`, `timeout`, `reminders` |
| `artifact.generate` | Create/refresh an artifact version | `artifactType`, `mode` (`mapping`, `ai`, `hybrid`), `mapping`, `aiTask` |
| `actions.sync` | Create/update Action Items from entities | `fromTypes`, `trigger` |
| `notify` | Emit a notification type | `type`, `recipients` |
| `native.*` | Official native handler | per handler |

Control features: `needs` (DAG edges), `when` (JSON Logic on job input/config), `continueOnError` (optional steps), `parallel` batching inside `ai.extract`.

## Rules

- The DAG must be acyclic; every `ai.*` task references a manifest `ai.tasks` key; every produced type exists in `knowledge.entityTypes`.
- Definitions are pinned per run (hash stored on ProcessingRun).
- Approval workflows use the separate format in [approval](../../platform/collaboration/approval.md).

## Open

Final DSL syntax and expression language — [OD-018](../../governance/open-decisions.md#od-018). The Hello Module uses a minimal subset (`ingest.require` → `ai.extract` (mock) → `artifact.generate`).
