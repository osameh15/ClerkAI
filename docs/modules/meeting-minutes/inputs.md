# Meeting Minutes — Inputs

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Module Platform |
| Blueprint | BP §3, §3.1 |

## Slots

| Slot | Accepts | Required extractions | Capture | Multiple |
|---|---|---|---|---|
| `recording` | audio, video | normalized_audio, transcript, diarization, language | upload, record_audio, record_video | ✓ (segments of one meeting) |
| `agenda` | document, image, text | text, layout | upload, camera, text | ✗ |
| `notes` | image, document, text | text, handwriting_ocr | upload, camera, scan, text | ✓ |
| `attachments` | document, presentation, spreadsheet, image | text, tables | upload | ✓ |

Readiness: at least one of `recording`, `notes`, `agenda` (minutes from notes-only meetings are allowed). Limits: 30 sources, 6 h total media, 300 pages.

## Job Input (`schemas/job-input.schema.json`)

| Field | Type | Notes |
|---|---|---|
| `meeting_title` | string | Defaults to job title |
| `meeting_date` | dateValue | Default: recording capture date |
| `location` | string | Optional |
| `participants[]` | `{ name, membership_id?, role_text?, organization? }` | Greatly improves attribution; members auto-suggested |
| `chair` | person ref | Optional |
| `language` | `auto`/`fa`/`en`/`mixed` | Routing hint |
| `glossary[]` | strings | Names/terms to bias ASR |
| `recording_notice_confirmed` | boolean | Required `true` when a recording is attached and org policy requires notice ([OD-032](../../governance/open-decisions.md#od-032)) |

## Multiple Recordings

Multiple recordings are ordered by `position`/capture time and treated as one timeline with offsets; evidence pointers reference the specific source.

## Live Recording

Browser MediaRecorder → local chunks in IndexedDB → multipart upload in background → Source created at stop. Interruptions keep local chunks for retry ([upload-queue](../../pwa/upload-queue.md)).
