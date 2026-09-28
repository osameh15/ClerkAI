# Lecture Summary — Workflow

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (DSL Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + AI Engineer |
| Blueprint | BP §3.4, App. A (Lecture Summary Flow) |

## Processing DAG (`lecture-processing`)

```mermaid
flowchart LR
  S[ingest.require<br/>transcript, frames, slides,<br/>handout text/OCR] --> AL[ai.extract: align_slides<br/>slide ↔ transcript segments]
  AL --> F[knowledge.fuse<br/>temporal + semantic]
  F --> C[ai.extract: extract_concepts<br/>topics, concepts, definitions,<br/>examples, key points, questions,<br/>formulas, relations]
  C --> RS[knowledge.resolve<br/>concept canonicalisation<br/>fa/en aliases]
  RS --> G[review.gate<br/>blocking_only]
  RS --> LS[artifact.generate: lecture_summary<br/>hybrid]
  G --> W[review.await]
  LS --> W
```

Long lectures: `extract_concepts` batches by topic segments using retrieval to keep prompts within limits ([retrieval](../../knowledge/retrieval.md)).

## Additional Artifacts

Study notes, flashcards and question sets via `lecture-additional-artifact` (`ai.generate` with `generate_study_material` task) from existing Knowledge; no re-transcription.

## Approval

None by default (education use). Organizations (training departments) may attach an approval workflow via configuration if entitled.
