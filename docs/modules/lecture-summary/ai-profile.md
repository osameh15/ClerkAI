# Lecture Summary — AI Profile

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §3.4, §11 |

## Tasks

| Task | Capability | Profile | Prompt | Notes |
|---|---|---|---|---|
| `transcribe` | transcription | `persian_transcription` | — | Technical vocabulary; code-switching |
| `align_slides` | structured_extraction | `standard_extraction` | `lecture.slide-alignment@^1.0.0` | Uses frame OCR + slide text + transcript |
| `extract_concepts` | structured_extraction | `high_quality_extraction` | `lecture.concept-extraction@^1.0.0` | Citations mandatory |
| `generate_study_material` | generation | `standard_generation` | `lecture.study-material@^1.0.0` | Grounded; flags out-of-source |

## AI Risk Profile

| Aspect | Assessment |
|---|---|
| Intended use | Study aid |
| Impact of errors | Learning incorrect facts |
| Highest risk | Hallucinated definitions/answers; mistranscribed technical terms/formulas |
| Mitigations | Source-grounded mode; citation verification; formula threshold; user review; labelled enrichment |
| Languages | Persian + English technical terms; mixed slides |
