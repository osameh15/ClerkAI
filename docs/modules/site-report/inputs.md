# Site Report — Inputs

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Frontend |
| Blueprint | BP §3.2, §21 |

## Slots

| Slot | Accepts | Required extractions | Capture | Offline |
|---|---|---|---|---|
| `observation_media` | image, video | metadata, vision_caption, ocr, frames | camera, record_video, upload | ✓ |
| `voice_notes` | audio | normalized_audio, transcript, language | record_audio, upload | ✓ |
| `text_notes` | text | text | text | ✓ |
| `reference_documents` | document, image | text, layout | upload | ✗ |

Readiness: ≥ 1 of `observation_media`, `voice_notes`, `text_notes`. Limits: 300 sources, 5 GB, 2 h audio/video.

## Capture Groups

The PWA records a `capture_group_id` (client-generated) for sources captured within one "observation session" (for example photo + voice note describing it) plus `captured_at` and optional GPS. These travel as upload metadata (`client_ref`, `capture_group` in source metadata) and are used by fusion (`capture_group` strategy) — explicit grouping always overrides AI grouping.

## Job Input

| Field | Notes |
|---|---|
| `project_id` | Required when `requireProject=true` (default) |
| `visit_date`, `visit_time_window` | Default from captures |
| `site_area` / `zones[]` | Optional location vocabulary for the visit |
| `weather`, `attendees[]`, `contractors_present[]` | Optional report header data |
| `inspection_type` | enum: routine, quality, safety, handover (config vocabulary) |
| `previous_visit_job_id` | Optional explicit repeat-inspection base (else latest final visit in project) |
