# Embeddings

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline (model Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §6 (pgvector initial), App. E |

## Decisions

- Chunk embeddings computed via the AI Gateway `embedding` capability (profile `multilingual_embedding`) so provider/model can change without module changes.
- Stored in `chunk_embeddings` with `embedding_model` identifier ([vector-storage](../data/vector-storage.md)).
- Computed lazily: only when a module step needs retrieval or project-scope matching, or when org policy enables eager embedding; cost metered.
- Model change → background re-embedding per organization, old vectors deleted after switch-over.
- Embeddings are derived content: deleted with sources; classified Confidential (embeddings can leak content).

## TBD

Model and dimension: [OD-039](../governance/open-decisions.md#od-039) — requires Persian retrieval evaluation.
