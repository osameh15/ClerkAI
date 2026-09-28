# Image Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain + AI Engineer |
| Blueprint | BP §9 ("vision → OCR/handwriting → layout → bounding boxes"), §3.2 (site photos) |

## Pipeline

| Step | Output |
|---|---|
| Decode + validate (size/megapixel limits) | `metadata` (dimensions, EXIF: capture time, GPS, orientation) |
| Normalise | Orientation fix, HEIC→JPEG rendition, thumbnail, web preview |
| Classification | Photo vs document/scan vs whiteboard vs handwriting (cheap classifier or vision call) — routes the next steps |
| OCR (printed) | `ocr` with word/line bounding boxes |
| Handwriting | `handwriting_ocr` ([handwriting-processing](handwriting-processing.md)) |
| Layout | `layout` regions (text blocks, tables, figures) with boxes |
| Scene understanding | `vision_caption`: objects, conditions, visible text, safety-relevant elements (for site photos) |

## Rules

- All regions carry normalised bounding boxes (0–1) for evidence ([evidence.schema.json](../api/schemas/evidence.schema.json)).
- EXIF GPS is metadata, never shown externally without permission; stripped from exported renditions unless the module explicitly includes location.
- Photos are not "documents": scene understanding output is descriptive and module steps decide what becomes Knowledge (for example an Observation).
