# Extraction Cache

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §9 ("reused rather than paid for repeatedly"), §22.1 ("Deduplication and reuse of Source processing", "Caching deterministic extractions") |

## Rule

An extraction is reused when `(source_id, kind, pipeline_version, options_hash)` matches an existing `succeeded` extraction. `options_hash` covers parameters that change output (for example language hint, diarization speaker count hint).

## Cross-Source Reuse (dedup)

When two Sources in the **same organization** share SHA-256 ([deduplication](deduplication.md)), extractions may be copied by reference (new rows pointing to the same payload object) — no recomputation, no extra usage charge. Never across organizations ([tenancy-architecture](../architecture/tenancy-architecture.md#5-cross-tenant-scenarios-explicitly-forbidden)).

## Invalidation

| Trigger | Behaviour |
|---|---|
| New pipeline version | Old extraction stays valid for existing evidence; new Jobs requiring the new version trigger re-extraction |
| Model profile route change without output-format change | No invalidation (pipeline version unchanged) — quality changes are managed through evaluation and version bumps |
| Human correction of transcript/OCR | Stored as Knowledge corrections, not by mutating extractions |
| Source deletion | Extractions deleted with the source |
| Retention of derived content | Extractions deleted per policy; re-computable while the original exists |

## Non-Deterministic Outputs

AI extractions are not strictly deterministic; caching guarantees **consistency** (same Source → same transcript across Jobs) which is required for stable evidence, not bit-exact reproducibility.
