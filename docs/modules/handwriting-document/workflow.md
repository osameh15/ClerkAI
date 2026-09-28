# Handwriting → Document — Workflow

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (DSL Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + AI Engineer |
| Blueprint | BP §3.3, App. A (Handwriting Flow) |

## Processing DAG (`handwriting-processing`)

```mermaid
flowchart LR
  S[ingest.require<br/>handwriting_ocr, ocr, layout per page] --> ST[ai.extract: structure_text<br/>blocks, reading order,<br/>persons, dates, amounts, tasks]
  ST --> RS[knowledge.resolve<br/>dates, amounts normalisation]
  RS --> G[review.gate<br/>always for uncertain spans;<br/>beforeArtifacts: true]
  G --> W[review.await]
  W --> CD[artifact.generate: clean_document<br/>mapping]
```

`requireReviewBeforeArtifact: true` — the clean document is generated after uncertain spans are resolved, so the first artifact version contains no guessed text.

## Additional Artifacts (`handwriting-additional-artifact`)

User chooses Formal Letter / Meeting Minutes / Task List → `POST /v1/jobs/{id}/artifacts` → run with `ai.generate` (`compose_artifact`) or mapping → new Artifact in the same Job. Several artifacts can coexist.

## Approval

`letter-approval` for formal letters (configurable; default off). Clean documents and task lists do not require approval by default.
