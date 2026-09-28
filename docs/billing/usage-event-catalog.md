# Usage Event Catalog (Meters)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce domain |
| Blueprint | BP §22 (Meter / Internal Unit table) |

## Meters (canonical)

| Meter (BP §22) | Internal unit | Emitted by | Attribution | Customer unit mapping |
|---|---|---|---|---|
| `audio.transcription` | seconds | AI Gateway (transcription/diarization) | source, job | Processing Minutes (+ AI Credits if rate card says) |
| `video.processing` | seconds + sampled frames (`frames` sub-quantity) | Ingestion | source, job | Processing Minutes |
| `vision` | images / pages | AI Gateway (vision) | source, job | AI Credits |
| `ocr` | pages | AI Gateway / OCR engine | source, job | Pages (+ AI Credits) |
| `llm.input` | tokens | AI Gateway | ai_execution, job, module | AI Credits |
| `llm.output` | tokens | AI Gateway | ai_execution, job, module | AI Credits |
| `embedding` | tokens | AI Gateway | source | AI Credits (or absorbed) |
| `storage` | byte-hours | Storage accounting (daily snapshot × 24) | organization, source | Storage GB-month |
| `artifact.export` | count / pages | Export worker | artifact | Included / count |
| `module.execution` | count | Job completion | job, module | Informational / module pricing |
| `api` | requests / usage class | API gateway middleware (API keys) | api key | API volume tiers |

`embedding` is a refinement (not in BP §22); it may be absorbed by pricing but is always recorded for cost attribution.

## Naming

Meters are lowercase dot-separated; sub-quantities go in `attributes` (for example `{"frames": 120}`). New meters require an update here and rate-card support.
