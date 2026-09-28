# PDR-004: A Universal Source Can Be Reused by Multiple Artifacts and Modules

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Product |
| Blueprint | BP §1.1 Source-first, §9, §28.2 |

## Decision

A Source belongs to an Organization, not to a Job or Module. Users can upload to the Inbox without choosing a module and later attach the same Source to multiple Jobs. Source-level preprocessing (OCR, transcription, parsing) is cached as Extractions and reused rather than paid for again.

## Consequences

- Data model: `sources` ⟂ `jobs` via `job_sources`.
- Access: a user attaching a Source must be authorised to read it.
- Deleting a Source referenced by final Artifacts follows retention policy (Evidence may become "source unavailable" rather than breaking the Artifact).
- Handwriting: one Source can produce Clean Document, Letter, Minutes, Task List.

## Related

[source-model](../../ingestion/source-model.md), [deduplication](../../ingestion/deduplication.md), [extraction-cache](../../ingestion/extraction-cache.md)
