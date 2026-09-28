# Source Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §9 |

## SourceIngestionWorkflow

- ID: `source/{source_id}/ingest/{pipeline_set_version}`; queue `general`, activities on `media`, `document`, `ai`.
- Steps: `verify_and_promote` → `sniff` → `scan` → `route` → per-kind activities (parallel where independent) → `normalize` → `chunk` → `embed` (policy) → `complete`.
- On-demand kinds requested by Jobs start a child workflow `source/{source_id}/extract/{kind}@{version}` (idempotent by ID).

## Pipeline Registry

Each pipeline kind is registered with: `pipeline_key`, `version`, input kinds, dependencies (for example `diarization` depends on `normalized_audio`), queue, timeouts, retry policy, AI capability profile (if any), cost meter.

| Pipeline key | Depends on | Queue | AI profile |
|---|---|---|---|
| `metadata` | original | media/document | — |
| `normalized_audio` | original (audio/video) | media | — |
| `transcript` | normalized_audio | ai | `persian_transcription` / language-routed |
| `diarization` | normalized_audio | ai | `speaker_diarization` |
| `language` | transcript/text | general | — (library) or `language_detection` |
| `frames` | original video | media | — |
| `vision_caption` | image / frames | ai | `high_quality_vision` |
| `ocr` | image / scanned page | ai or document | `document_ocr` |
| `handwriting_ocr` | image / scanned page | ai | `persian_handwriting_ocr` |
| `layout` | pdf / image | document | — or `layout_analysis` |
| `text` | pdf text layer / office / text | document | — |
| `tables` | pdf / office / spreadsheet | document | — |
| `slides` | pptx / pdf-as-slides | document | — |
| `normalized_content` | all available kinds | general | — |
| `embeddings` | chunks | ai | embedding profile ([OD-039](../governance/open-decisions.md#od-039)) |

## Versioning and Invalidation

A pipeline version bump (new model profile route that changes output format, new parser) does **not** automatically re-run existing sources. Re-extraction happens on demand when a Job requires the new version (module declares minimum pipeline versions) or via an admin backfill job. See [extraction-cache](extraction-cache.md).

## Observability

Per-kind duration, failure rate, cost per source; `source_id` and `pipeline_key` as span attributes.
