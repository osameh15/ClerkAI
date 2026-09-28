# Fine-Tuning Decision Framework

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Technical Leadership |
| Blueprint | BP §11.5 |

## Criteria (all required, BP §11.5)

| # | Criterion | Evidence required |
|---|---|---|
| 1 | Error pattern is stable and prompt/RAG techniques are no longer sufficient | Failure taxonomy trend over ≥ 2 releases; documented prompt/retrieval attempts |
| 2 | At least hundreds or thousands of reviewed examples | Approved dataset size per task |
| 3 | Measurable improvement on an independent evaluation set | Held-out golden set results vs current route |
| 4 | Real economic advantage (cost, latency, privacy, consistency) | Unit economics model: cost per accepted artifact before/after incl. training and hosting |
| 5 | Training data usable under consent, ownership, retention policy | Consent snapshot, legal review |

## Process

Proposal (ADR) → dataset freeze → training (approved provider/infra) → evaluation vs production routes → security/privacy review (model card: data cutoff, consent basis, known limits) → add as profile route behind flag → canary → promotion via [model-change-management](../../governance-ai/model-change-management.md).

## Candidate Areas (hypotheses)

Persian handwriting OCR; Persian meeting action extraction; construction observation categorisation.
