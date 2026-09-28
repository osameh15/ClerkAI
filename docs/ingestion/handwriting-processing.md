# Handwriting Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (providers Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain + AI Engineer |
| Blueprint | BP §3.3, §9, App. D ("Uncertain text", "No silent invention") |

## Pipeline

1. **Page preparation:** deskew, perspective correction (camera captures), contrast normalisation, page splitting for multi-page PDFs/TIFFs; each page becomes a page image rendition.
2. **Handwriting recognition:** `handwriting_recognition` capability (profile `persian_handwriting_ocr` or language-routed), returning lines/words with bounding boxes, confidence and alternatives.
3. **Mixed content:** printed text regions via `ocr`; both merged into layout order.
4. **Uncertainty marking:** tokens below the legibility threshold are flagged `uncertain` with alternatives; unreadable regions are flagged `unreadable` (no text invented).
5. **Structure:** reading order, headings, list items, tables — produced by the layout step and refined by module structuring prompts.

## Guarantees

- The extraction never replaces unreadable content with guessed text; the module must surface such spans as Review Items and block finalisation until resolved (configurable only to "mark as [illegible]" explicitly, never silent).
- Numbers and dates carry stricter thresholds (module configuration) because misreadings are high-impact.

## Evaluation

CER per language, uncertain-span recall, numbers/dates accuracy ([handwriting-document evaluation-plan](../modules/handwriting-document/evaluation-plan.md)).
