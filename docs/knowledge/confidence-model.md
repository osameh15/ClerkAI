# Confidence Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain + AI Engineer |
| Blueprint | BP §1.1 (Human-in-the-loop), §10, §16.2 ("Ambiguous information displays confidence warnings") |

## Principle

**Raw model-generated confidence is not assumed calibrated.** Confidence is a routing signal for human review; its trustworthiness is validated by evaluation.

## Components

| Component | Definition |
|---|---|
| Raw confidence | Score in [0,1] from the source: ASR/OCR token confidences (aggregated), model self-reported confidence (structured output field), or heuristic signals |
| Composite confidence | Platform-computed score per item: `f(raw, evidence_quality, agreement_across_sources, extraction_quality)` — v1 uses a documented weighted rule: min(raw, evidence factor), boosted when ≥ 2 independent sources agree, penalised when evidence missing or contradicting |
| Calibrated confidence | Composite mapped through a per-(module, entity type, profile) calibration table learned from evaluation data (isotonic/Platt). Until calibration data exists, identity mapping is used and flagged `calibrated=false` |
| Band | `high` / `medium` / `low` from module thresholds |

## Bands and Thresholds

Defaults (provisional, [OD-022](../governance/open-decisions.md#od-022)): `high ≥ 0.85`, `medium ≥ 0.60`, `low < 0.60`. Modules override in `review.bands`; entity types add `review.confidenceThreshold` (item below threshold → Review Item). Organizations may raise (never lower below module minimum) thresholds in module configuration.

| Band | UX | Review |
|---|---|---|
| High | No badge (or subtle) | None unless `alwaysReview` |
| Medium | Amber ConfidenceBadge | Review Item `warning` if below entity threshold |
| Low | Red ConfidenceBadge + "verify" | Review Item; `blocking` if the type blocks finalisation |

## Review Triggers

Below threshold; missing required evidence; conflicting sources; ambiguous attribution; unreadable spans; policy `alwaysReview`.

## Human Confirmation and Recalculation

- `human_confirmed` or `human_modified` items have effective confidence 1.0 for artifact purposes (raw AI confidence retained for evaluation).
- Recalculation happens when new evidence is attached, a contradicting source is added (new run), or calibration tables change (affects future runs only; stored values carry `calibration_version`).

## Validation of Confidence (evaluation)

Evaluation reports **Expected Calibration Error** and reliability diagrams per module/entity type; release gates may require ECE below a threshold for types that skip review at high confidence ([evaluation-metrics](../ai/evaluation/evaluation-metrics.md)). If calibration is poor, thresholds are raised (more review) rather than trusting scores.
