# Handwriting → Document — AI Profile

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §3.3, §11 |

## Tasks

| Task | Capability | Profile | Prompt | Notes |
|---|---|---|---|---|
| `recognize_handwriting` (ingestion-level) | handwriting_recognition | `persian_handwriting_ocr` | — | Returns lines/words, boxes, confidences, alternatives |
| `structure_text` | structured_extraction | `high_quality_extraction` | `handwriting.structure@^1.0.0` | Uses OCR text + page images; must not "correct" content beyond normalisation; flags uncertain spans |
| `compose_artifact` | generation | `standard_generation` | `handwriting.compose@^1.0.0` | For letter/minutes; binds facts to Knowledge IDs |

## AI Risk Profile

| Aspect | Assessment |
|---|---|
| Highest risk | Invented or altered text, numbers, dates, names in official documents |
| Mitigations | Uncertain-span model; review before artifact; numbers/dates stricter thresholds; letter facts must bind to Knowledge |
| Languages | Persian handwriting variability (Nastaliq-influenced styles, connected letters, dots), mixed with English/digits |
| Candidate for future fine-tuning | Persian handwriting OCR ([fine-tuning-decision-framework](../../ai/training/fine-tuning-decision-framework.md)) |
