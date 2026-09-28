# Lecture Summary — Evaluation Plan

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §19.1 ("Lecture: audio + slides, concept/definition/example extraction, and hallucination control"), App. D |

## Dataset

`lecture-summary-golden@1`: 30 lectures (≥ 20 Persian/mixed) with slides, from licensed/open courseware or recorded with lecturer consent; annotated concepts, definitions, examples, key points, relations, and slide–speech alignment; forbidden-claims lists for hallucination tests.

## Metrics

| Metric | Threshold |
|---|---|
| `concept.recall` (core concepts) | ≥ 0.80 |
| `definition.accuracy` | ≥ 0.85 |
| `evidence.coverage` (core concepts linked to transcript/slide evidence, App. D) | ≥ 0.90 |
| `hallucination.out_of_source_claim_rate` (source-grounded mode) | ≤ 0.03 |
| `flashcard.answerable_from_source_rate` | ≥ 0.95 |
| Slide alignment accuracy | reported |
| `transcription.wer_fa` on technical lectures | reported |

## Human Evaluation

Teaching assistants rate summaries and flashcards (accuracy, usefulness, coverage) for 10 lectures per release candidate.
