# Evidence Viewer UI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend |
| Blueprint | BP §16.2 ("Important facts expose an Evidence Viewer"), §20.2 (EvidenceViewer) |

## Behaviour per Pointer Type

| Pointer | Viewer |
|---|---|
| `time_range` | Audio/video player seeks to start; highlighted region on waveform/timeline; transcript segment highlighted; loop option; speaker label |
| `page` | PDF/slide viewer at page/slide; for spreadsheets, sheet + cell range highlight |
| `bbox` | Image/page with overlay rectangle; zoom-to-fit region; frame from video at `frame_ms` |
| `text_span` | Text view with highlighted span and surrounding context |
| `chunk` | Chunk text with source link |

## Rules

- Media loaded through short-lived presigned URLs (`GET /v1/sources/{id}/files/{fileId}/url`) after authorization; refreshed on expiry.
- Multiple evidence items: tabbed list with support type (supports/contradicts/context) and confidence.
- `source_unavailable`: metadata card explaining retention deletion.
- Accessibility: keyboard controls; captions from transcript for media.
