# Universal Input UI (Source Composer)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend |
| Blueprint | BP §20 (Quick Clerk, source-first), App. A (New Job, Source Composer), §20.2 (SourceUploader, AudioRecorder, CameraCapture, SourcePreview) |

## Composer

Renders module input slots from the manifest (`GET /v1/store/modules/{key}` → inputs) or generic "any source" for the Inbox.

| Capture mode | Component | Notes |
|---|---|---|
| `upload` | SourceUploader (drag/drop, picker, multi-file) | Multipart, resumable, per-file progress |
| `record_audio` | AudioRecorder | MediaRecorder; chunked local persistence; pause/resume; level meter |
| `record_video` | VideoRecorder (in AudioRecorder family) | Size warnings |
| `camera` | CameraCapture | Burst, retake, edge detection for documents |
| `scan` | CameraCapture (document mode) | Perspective correction preview |
| `text` | Manual note editor | ≤ 200k chars |
| Reuse | "Add from Inbox" picker | Attaches existing Sources (PDR-004) |

## Behaviour

- Slot validation client-side (kinds, limits) mirrors server; server errors mapped to slot messages.
- Readiness indicator: "Ready to submit" / missing requirements (from `Job.readiness`).
- Adding sources never auto-submits.
- Offline-capable slots queue locally ([upload-queue](../pwa/upload-queue.md)).
- Recording notice checkbox shown when module/org policy requires ([OD-032](../governance/open-decisions.md#od-032)).
- Cost estimate (credits/minutes) displayed before submit ([ai-credit-model](../billing/ai-credit-model.md)).
