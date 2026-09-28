# Handwriting → Document — Evaluation Plan

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §19.1 ("Handwriting: multiple image qualities, ambiguity handling, and review flow"), App. D |

## Dataset

`handwriting-golden@1`: 150 pages from ≥ 30 writers (volunteers, consented), Persian/English/mixed, qualities: flatbed scan, good phone photo, low light, skewed, crumpled, pencil, ruled paper, tables/lists, numbers and Jalali dates. Ground truth: transcription per line with boxes; legibility labels (clear/uncertain/illegible) agreed by two annotators.

## Metrics

| Metric | Threshold |
|---|---|
| `ocr.cer_fa` | ≤ 0.12 (clear-legibility lines) |
| `ocr.cer_en` | ≤ 0.08 |
| `uncertain.recall` (truly uncertain/illegible spans flagged) | ≥ 0.95 |
| `numbers_dates.accuracy` | ≥ 0.97 on confirmed-legible values |
| `hallucination.silent_invention_rate` | 0 |
| Structure accuracy (reading order, heading/list detection) | reported |
| Letter factual binding rate (facts bound to knowledge) | ≥ 0.99 |

## Review Flow Test

End-to-end test that every illegible span blocks finalisation until resolved (100%).
