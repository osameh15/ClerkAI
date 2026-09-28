# Multimodal Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §0, §3, §9, §11 ("multimodal context fusion") |

## Strategy

1. **Convert to text-first representations** during ingestion (transcripts, OCR, captions, slide text) because they are cheaper, cacheable, citable and evaluable.
2. **Use native multimodal calls selectively** where text loses essential information: site photos (conditions, defects), handwriting with layout, diagrams/slides with visuals, before/after comparisons.
3. **Mixed prompts** carry both text chunks and image references with handles (`[I3]`), so outputs can cite image regions.

## Modalities by Capability

| Capability | Inputs | Output |
|---|---|---|
| transcription / diarization | audio | segments with timestamps, speakers |
| vision_analysis | images, frames | structured observations, regions (bbox), captions |
| ocr / handwriting_recognition | page images | lines/words with boxes and confidences |
| structured_extraction | text chunks (+ images) | module schema with citations |
| generation / summarization | knowledge + chunks | schema-bound prose with citations |
| embedding | text | vectors |

## Cost Control

Image downscaling to model-appropriate resolution; frame sampling caps; batching pages; caching per Source ([extraction-cache](../ingestion/extraction-cache.md)).
