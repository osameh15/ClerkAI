# Meeting Minutes — Evaluation Plan

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §11.4, §19.1 ("Meeting: Persian owner/deadline/decision extraction with Evidence"), App. D |

## Datasets

| Dataset | Content | Size (initial) |
|---|---|---|
| `meeting-minutes-golden@1` | Staged/synthetic meetings with agenda + notes; annotated decisions/proposals/discussions/unresolved/actions/owners/deadlines/risks/questions + evidence spans | 60 meetings (≥ 40 Persian/mixed) |
| `meeting-audio-fa-en@1` | Audio with reference transcripts and speaker turns | 20 h |

Tags per [golden-datasets](../../ai/evaluation/golden-datasets.md): clean/poor audio, multi-speaker overlaps, missing deadlines, ambiguous owners, contradictory agenda vs speech, notes-only meetings, Jalali/relative dates.

## Metrics and Thresholds (release gate)

| Metric | Threshold |
|---|---|
| `decision.f1` | ≥ 0.80 |
| `action.precision` / `action.recall` | ≥ 0.85 / ≥ 0.80 |
| `owner.accuracy` | ≥ 0.85 |
| `deadline.accuracy` | ≥ 0.85 |
| `speaker.attribution_accuracy` | reported (target ≥ 0.85) |
| `evidence.coverage` | ≥ 0.90 |
| `evidence.alignment` | reported (target ≥ 0.80) |
| `hallucination.critical_unsupported_action_rate` | < 0.01 |
| `schema.validity_after_retry` | ≥ 0.995 |
| `transcription.wer_fa`, `proper_noun_accuracy`, `diarization.der` | reported |
| Ambiguous speaker/owner → review item (not guess) | 100% of tagged cases |

## Failure Taxonomy Focus

`wrong_type` (proposal counted as decision), `wrong_attribution`, `date_calendar_error`, `missed_item` in overlapping speech, `unsupported_claim`.

## Human Evaluation

Blind rubric scoring of minutes (formality, completeness, correctness) for 20 meetings per release candidate; secretaries/PMs as raters.
