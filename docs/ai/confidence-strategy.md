# Confidence Strategy (AI Side)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §10, §16.2 |

## Purpose

How the AI layer produces raw confidence signals. Banding, thresholds and calibration are canonical in [confidence-model](../knowledge/confidence-model.md).

## Signals Produced

| Source | Signal |
|---|---|
| ASR | Word/segment confidences (provider) → segment mean and min |
| OCR/handwriting | Token confidences, legibility flags, alternatives |
| LLM extraction | Self-reported `confidence` per item (schema field) + `uncertainty_reason`; never trusted alone |
| Agreement | Multiple sources or multiple samples agree (self-consistency for high-risk fields, optional and cost-controlled) |
| Evidence strength | Citation present and quote verified; number of independent sources |

## Rules

- Self-reported LLM confidence is combined with objective signals; high self-confidence with missing evidence is treated as low.
- Prompts instruct models to mark uncertainty explicitly and to prefer `null` + `uncertainty_reason` over guessing (ties to "no silent invention").
- Confidence outputs are part of evaluation (calibration metrics) so that thresholds are data-driven.
