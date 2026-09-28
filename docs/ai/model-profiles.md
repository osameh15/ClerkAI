# Model Profiles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (routes Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11.1, §11.2 ("model_profiles: abstract aliases such as persian_transcription or high_quality_vision") |

## Definition (canonical)

A **Model Profile** is an abstract, versioned alias that modules reference. It defines capability, quality tier, languages, modalities and an ordered set of **routes** to concrete models with conditions. Modules never reference models ([ADR-017](../governance/adr/ADR-017-provider-neutral-ai.md)).

## v1 Profile Catalogue

| Profile key | Capability | Quality | Languages | Modalities | Used by |
|---|---|---|---|---|---|
| `persian_transcription` | transcription | high | fa, en, mixed | audio | Meeting, Site, Lecture |
| `english_transcription` | transcription | standard | en | audio | Language-routed |
| `speaker_diarization` | diarization | standard | any | audio | Meeting |
| `high_quality_extraction` | structured_extraction | high | fa, en | text (+image) | All modules |
| `standard_extraction` | structured_extraction | standard | fa, en | text, image | Lecture alignment |
| `standard_generation` | generation / summarization | standard | fa, en | text | Summaries, letters, study material |
| `high_quality_vision` | vision_analysis | high | fa, en | image, video frames | Site Report |
| `document_ocr` | ocr | standard | fa, en | image | Ingestion |
| `persian_handwriting_ocr` | handwriting_recognition | high | fa, en | image | Handwriting, notes |
| `layout_analysis` | ocr (layout) | standard | any | image, pdf | Ingestion |
| `language_detection` | classification | economy | any | text | Ingestion |
| `multilingual_embedding` | embedding | standard | fa, en | text | Retrieval |

## Route Definition (`model_profile_routes`)

```yaml
profile: high_quality_extraction
version: 3
routes:
  - priority: 1
    model: providerA/model-x-2026-07
    conditions: { languages: [fa, en], max_input_tokens: 180000, regions: [any] }
    timeout_ms: 90000
    max_cost_micros: 400000
  - priority: 2
    model: providerB/model-y-2026-05
    conditions: { languages: [fa, en] }
    timeout_ms: 90000
  - priority: 3
    model: local/open-weight-z
    conditions: { tenant_policy: [private_ai_required] }
```

(Model names are placeholders; actual routes decided by evaluation, [OD-016](../governance/open-decisions.md#od-016).)

## Rules

- Profile semantics are stable; route changes are **model changes** and require evaluation + change record ([model-change-management](../governance-ai/model-change-management.md)).
- Profile `version` increments on any route change; AIExecutions record profile key + version + resolved model.
- Tenant policy can exclude routes (provider/region restrictions); if no route remains → `AI_POLICY_BLOCKED`.
