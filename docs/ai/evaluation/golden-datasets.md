# Golden Datasets

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (sizes Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Product |
| Blueprint | BP §19.1, App. D |

## Purpose

A golden dataset is the release-gating dataset per module: representative, adjudicated, versioned. Golden test execution is described in [testing/golden-tests](../../testing/golden-tests.md).

## Coverage Matrix (required tags per module)

| Scenario tag | Meeting | Site Report | Handwriting | Lecture |
|---|---|---|---|---|
| Persian content | ✓ | ✓ | ✓ | ✓ |
| English content | ✓ | ✓ | ✓ | ✓ |
| Mixed Persian/English | ✓ | ✓ | ✓ | ✓ |
| Clean audio | ✓ | ✓ (voice notes) | — | ✓ |
| Poor audio (noise, far-field, overlap) | ✓ | ✓ (site noise) | — | ✓ |
| Multiple speakers | ✓ | — | — | ✓ (Q&A) |
| Handwriting | ✓ (notes) | ✓ (site notes) | ✓ | ✓ (handouts) |
| Low-quality scans/photos | ✓ | ✓ | ✓ | ✓ |
| Contradictory evidence | ✓ (agenda vs speech) | ✓ (voice vs photo) | — | ✓ (slide vs speech) |
| Missing deadlines | ✓ | ✓ | ✓ (tasks) | — |
| Ambiguous owners | ✓ | ✓ (responsible party) | ✓ | — |
| Unsupported inputs | ✓ | ✓ | ✓ | ✓ |
| Provider failures (simulated) | ✓ | ✓ | ✓ | ✓ |
| Repeat inspection | — | ✓ | — | — |
| Formulas/diagrams | — | — | — | ✓ |
| Jalali dates / Persian digits | ✓ | ✓ | ✓ | — |

## Initial Size Targets (provisional)

| Module | Golden (gate) | Smoke subset (PR) |
|---|---|---|
| Meeting Minutes | 60 meetings (≥ 40 Persian/mixed) | 12 |
| Site Report | 50 visits (≥ 300 observations, 10 repeat-inspection pairs) | 10 |
| Handwriting | 150 pages (varied writers/quality) | 20 |
| Lecture Summary | 30 lectures (audio+slides) | 8 |

Sizes grow with the flywheel; dataset versions are pinned by module evaluation suites (`dataset@^1.0.0`).

## Creation Plan

G0–G1: annotation guidelines; G6–G8: first 30% of cases (synthetic/staged); G11: full golden sets before module release gates.
