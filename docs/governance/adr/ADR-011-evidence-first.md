# ADR-011: Evidence = First-Class Entity

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §1.1, §10, §28.1, App. D |

## Context

Trust in AI-generated business documents depends on the ability to open the exact source fragment behind a fact. Evidence rendered only in the UI cannot be validated, evaluated, or preserved across versions.

## Decision

- Evidence is persisted as structured rows (`evidence_links`) with typed pointers: audio/video time range, document page, image bounding box, text span, source chunk ([evidence.schema.json](../../api/schemas/evidence.schema.json)).
- Evidence links a **Knowledge Entity, Relation, or Artifact field path** to a **Source + Extraction** (so the pointer remains valid when extractions are re-run under a new version).
- Evidence rows are immutable; corrections add new rows and mark old ones `superseded`.
- Module Knowledge schemas declare which fields require Evidence. Missing required Evidence produces a Review Item.
- Evaluation measures evidence coverage and citation accuracy (target ≥ 90% openable evidence for key Meeting facts, BP App. D).

## Consequences

Model prompts must request and return source references (chunk IDs / timestamps / regions); the gateway validates they reference chunks that were actually provided (anti-hallucination).

## Related

[evidence-model](../../knowledge/evidence-model.md), [evidence-display](../../artifacts/evidence-display.md), [hallucination-controls](../../ai/hallucination-controls.md)
