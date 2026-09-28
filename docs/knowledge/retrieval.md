# Retrieval

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain + AI Engineer |
| Blueprint | BP §14.3 ("RAG retrieval is tenant-scoped and permission-aware"), §6 (pgvector) |

## Uses in v1

1. **Context selection for long inputs** — when a job's content exceeds a model's effective context, the extraction step retrieves relevant chunks per sub-task (for example agenda item) instead of sending everything.
2. **Cross-source alignment** (semantic strategy in [context-fusion](context-fusion.md)).
3. **Repeat inspection** matching of new observations to prior open issues within a project (Site Report).

## Rules

- Retrieval scope is always explicit: `job` (default), `project` (module-declared), never organization-wide in v1.
- Filter first (organization, scope, source access), then rank (vector similarity + lexical for Persian robustness), then optionally rerank.
- Retrieved chunks keep their handles for citation; retrieval never fabricates text.
- Retrieval results are logged by chunk IDs (not content) on the AIExecution for provenance.

## Open

Search implementation and hybrid ranking: [OD-031](../governance/open-decisions.md#od-031); embedding model: [OD-039](../governance/open-decisions.md#od-039).
