# Vector Storage

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline (model/dimension Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Tech Lead |
| Blueprint | BP §6, App. E |

## Decision

pgvector in PostgreSQL ([ADR-019](../governance/adr/ADR-019-pgvector-initial.md)).

## Table

`chunk_embeddings(chunk_id PK/FK, organization_id, embedding_model, dims, embedding vector(dims), created_at)` with RLS. One row per chunk per active embedding model; model migration adds rows for the new model then deletes old rows.

## Usage

- Retrieval for long Sources (lecture transcripts, multi-document meetings) to select relevant chunks for extraction prompts ([retrieval](../knowledge/retrieval.md)).
- Always filtered by `organization_id` and by Job/Project scope **before** similarity ordering.
- Not used for cross-tenant anything.

## Indexing

No ANN index until measured: exact search is acceptable while per-job candidate sets are small (the common case: a Job's own chunks). Add HNSW when p95 retrieval latency for org-wide search exceeds target on real data (BP App. E).

## Open

Embedding model and dimension: [OD-039](../governance/open-decisions.md#od-039). Persian retrieval quality must be evaluated.
