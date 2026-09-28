# Module Evaluations

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §11.4, §19.1, App. D |

## Summary per Module

Full plans: each module's `evaluation-plan.md`. Thresholds below equal the manifest suites and are provisional until real Persian datasets exist ([OD-022](../../governance/open-decisions.md#od-022)).

### Meeting Minutes — [plan](../../modules/meeting-minutes/evaluation-plan.md)

| Metric | Threshold | Blocking |
|---|---|---|
| `decision.f1` | ≥ 0.80 | Yes |
| `action.precision` / `action.recall` | ≥ 0.85 / ≥ 0.80 | Yes |
| `owner.accuracy` | ≥ 0.85 | Yes |
| `deadline.accuracy` | ≥ 0.85 | Yes |
| `speaker.attribution_accuracy` | report (target ≥ 0.85) | No (v1) |
| `evidence.coverage` | ≥ 0.90 | Yes |
| `hallucination.critical_unsupported_action_rate` | < 0.01 | Yes |
| `schema.validity_after_retry` | ≥ 0.995 | Yes |
| `transcription.wer_fa` | report | No |

### Site Report — [plan](../../modules/site-report/evaluation-plan.md)

| Metric | Threshold | Blocking |
|---|---|---|
| `observation.f1` | ≥ 0.80 | Yes |
| `observation.media_linkage_accuracy` | ≥ 0.90 | Yes |
| `category.accuracy` / `location.accuracy` | ≥ 0.80 | Yes |
| `severity.accuracy` | ≥ 0.75 | Yes |
| `corrective_action.recall` | ≥ 0.75 | Yes |
| `evidence.coverage` | ≥ 0.90 | Yes |
| `hallucination.unsupported_observation_rate` | ≤ 0.02 | Yes |
| `reinspection.match_accuracy` | ≥ 0.85 | Yes |

### Handwriting → Document — [plan](../../modules/handwriting-document/evaluation-plan.md)

| Metric | Threshold | Blocking |
|---|---|---|
| `ocr.cer_fa` / `ocr.cer_en` | ≤ 0.12 / ≤ 0.08 | Yes |
| `uncertain.recall` | ≥ 0.95 | Yes |
| `numbers_dates.accuracy` | ≥ 0.97 | Yes |
| `hallucination.silent_invention_rate` | 0 | Yes |

### Lecture Summary — [plan](../../modules/lecture-summary/evaluation-plan.md)

| Metric | Threshold | Blocking |
|---|---|---|
| `concept.recall` | ≥ 0.80 | Yes |
| `definition.accuracy` | ≥ 0.85 | Yes |
| `evidence.coverage` | ≥ 0.90 | Yes |
| `hallucination.out_of_source_claim_rate` | ≤ 0.03 | Yes |
| `flashcard.answerable_from_source_rate` | ≥ 0.95 | Yes |
