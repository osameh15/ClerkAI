# ADR-019: pgvector for Initial Embeddings

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership, AI Engineer |
| Blueprint | BP §6, App. E |

## Decision

Embeddings for Source Chunks (and optionally Knowledge Entities) are stored in PostgreSQL using **pgvector**, in tenant-scoped tables protected by RLS. Vector indexes (HNSW/IVFFlat) are created only after measuring real dataset size (BP App. E). Retrieval always filters by `organization_id` and authorization scope before similarity ranking. A `VectorIndex` port allows later migration to a dedicated engine.

## Consequences

Embedding model and dimension are [OD-039](../open-decisions.md#od-039); re-embedding on model change is a background job keyed by `embedding_model_version`.

## Related

[vector-storage](../../data/vector-storage.md), [retrieval](../../knowledge/retrieval.md)
