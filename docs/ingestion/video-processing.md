# Video Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (frame strategy Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain + AI Engineer |
| Blueprint | BP §9 ("metadata → audio extraction → transcription → frame sampling → visual analysis"), §22 (video meter) |

## Pipeline

| Step | Output |
|---|---|
| Metadata (ffprobe) | duration, resolution, codecs, rotation, capture time/location (if present) |
| Audio extraction | `normalized_audio` rendition → [audio pipeline](audio-processing.md) |
| Thumbnail + poster frame | renditions |
| Frame sampling | `frames` extraction: sampled frame images with timestamps ([OD-027](../governance/open-decisions.md#od-027): fixed rate vs scene change vs on-demand) |
| Visual analysis | `vision_caption` per sampled frame via `vision_analysis` capability; OCR on frames with text (slides in lecture video) |

## Cost Guardrails

- Default sampling is conservative (for example scene-change with max 1 frame / 10 s and a per-video cap), configurable per module (lecture videos sample slides; site videos sample on scene change).
- Meter `video.processing` = seconds + sampled frames (BP §22).
- Frames near evidence-relevant timestamps can be extracted on demand later.

## Evidence

Video evidence uses `time_range` (media `video`) and `bbox` with `frame_ms`.
