# Handwriting → Document — Inputs

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.3 |

## Slots

| Slot | Accepts | MIME narrowing | Required extractions | Capture |
|---|---|---|---|---|
| `pages` (required) | image, document | JPEG, PNG, HEIC, WebP, TIFF, PDF | metadata, handwriting_ocr, ocr, layout, language | camera, scan, upload |
| `context` | text | — | text | text |

Limits: 50 sources, 100 pages. Each PDF/TIFF page becomes a page image rendition; page order = source position then page number (user-reorderable before submit via job input `page_order`).

## Job Input

| Field | Notes |
|---|---|
| `page_order[]` | `{source_id, page}` ordering; excluded pages omitted |
| `document_language` | `auto`/`fa`/`en`/`mixed` |
| `intended_output` | Optional pre-selection (`clean_document`, `formal_letter`, `meeting_minutes`, `task_list`) |
| `letter_context` | For letters: recipient, subject hints (optional) |

## Capture Guidance

Camera overlay with page edge detection, glare warnings, auto-capture on stability; per-page quality score shown before upload (blur/lighting), allowing retake.
