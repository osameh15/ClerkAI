# Slide Processing (PPTX, PDF Slides)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §3.4 (slides + audio), §9 |

## Behaviour

- PPTX: per slide — title, body text (ordered shapes), tables, speaker notes, embedded images; slide render to image rendition (for evidence display and vision/OCR of text in images).
- PDF exported slides: detected by page aspect ratio/layout; treated as slides with page = slide number.
- Output `slides` extraction with `slide` locators and `text_span` offsets within slide text.
- Equations/formulas: captured as text when native; image equations via OCR with low-confidence flags.

## Alignment with Audio (Lecture)

Slide–transcript alignment is a **module step** (`education.lecture-summary` `align_slides` task) using timestamps from video frames (slide change detection) when available, otherwise semantic alignment. Ingestion only provides slides and transcript independently.
