# Site Report — Evaluation Plan

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §19.1 ("Site Report: image + voice, location/issue/action extraction, and repeat inspection"), App. D |

## Datasets

| Dataset | Content | Size |
|---|---|---|
| `site-report-golden@1` | Staged site visits (real sites with consent) — photos + voice notes + text; annotated observations, grouping, category, location, severity, corrective actions, responsible parties, evidence | 50 visits / ≥ 300 observations |
| `site-reinspection-golden@1` | Visit pairs with before/after photos and ground-truth outcomes | 10 project sequences |

Tags: noisy audio, Persian/mixed, low light photos, multiple issues per photo, voice describing multiple photos, missing responsible party, contradictory voice vs photo, repeat inspection resolved/persisting/worsened.

## Metrics

| Metric | Threshold |
|---|---|
| `observation.f1` | ≥ 0.80 |
| `observation.media_linkage_accuracy` (photo/voice attached to right observation, BP App. D) | ≥ 0.90 |
| `category.accuracy` | ≥ 0.80 |
| `location.accuracy` | ≥ 0.80 |
| `severity.accuracy` | ≥ 0.75; critical recall reported separately (target ≥ 0.95) |
| `corrective_action.recall` | ≥ 0.75 |
| `evidence.coverage` | ≥ 0.90 |
| `hallucination.unsupported_observation_rate` | ≤ 0.02 |
| `reinspection.match_accuracy` | ≥ 0.85 |
| Resolved proposals always produce review item | 100% |

## Human Evaluation

Site engineers rate report usefulness and correctness for 10 visits per release candidate.
