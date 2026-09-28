# Lecture Summary — Product Specification

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.4, App. A |

## Goal (BP §3.4)

Convert multimodal teaching material into educational Knowledge: concepts, definitions, examples, key points, questions, concept relationships and flashcards, with strong support for Persian and mixed slide-plus-audio content.

## Users

Students (often personal workspaces), lecturers/teaching assistants, corporate trainers (B2B training).

## Capabilities

| Capability | v1 |
|---|---|
| Inputs: audio, video, slides (PPTX/PDF), PDF handouts, images, text | ✓ |
| Slide–speech alignment | ✓ |
| Concepts, definitions, examples, key points, questions, formulas | ✓ |
| Concept relationships (prerequisite, part-of, related) | ✓ |
| Lecture summary (automatic) | ✓ |
| Study notes, flashcards, review questions (on demand) | ✓ |
| Source-grounded mode (default) flags out-of-source content | ✓ |
| Enrichment mode (explanations beyond source, clearly labelled) | Configurable, off by default |
| Anki/CSV flashcard export | Future (PDF print in v1) |
| Spaced-repetition scheduling | Future |

## Flow (App. A)

```mermaid
flowchart LR
  A[Lecture card] --> B[Upload/record lecture + slides/PDF]
  B --> C[Language / topic context]
  C --> D[Transcription + slide/document parse]
  D --> E[Concept fusion]
  E --> F[Summary + definitions +<br/>examples + questions]
  F --> G[Review / edit]
  G --> H[Generate flashcards / study notes]
  H --> I[Export / revisit]
```

## Grounding Policy

In `source_grounded` mode every definition, key point and flashcard answer must cite transcript or slide evidence (BP App. D "Concept grounding"); ungrounded content is hidden or marked "outside source" (BP App. D "Hallucination").
