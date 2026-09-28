# Lecture Summary — Inputs

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.4 |

## Slots

| Slot | Accepts | Required extractions | Capture |
|---|---|---|---|
| `lecture_media` | audio, video | normalized_audio, transcript, language, frames | upload, record_audio, record_video |
| `slides` | presentation, document | slides, text, layout | upload |
| `materials` | document, image, text | text, ocr, handwriting_ocr | upload, camera, text |

Readiness: ≥ 1 of the three slots. Limits: 40 sources, 4 h media, 500 pages.

## Job Input

| Field | Notes |
|---|---|
| `course` | Course name (optional) |
| `lecture_title`, `lecture_date`, `lecturer` | Optional metadata |
| `topic_context` | Short description to improve extraction (not treated as source facts) |
| `language` | `auto`/`fa`/`en`/`mixed` |
| `level` | enum: school, undergraduate, graduate, professional |
| `outputs` | Additional on-demand artifacts to generate immediately |

Video lectures: frames sampled on slide change when slides are visible in the video; aligned with uploaded slides if both exist.
