# Chunking

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain + AI Engineer |
| Blueprint | BP §7.3 (`source_chunks`), §9, §10 |

## Purpose

Chunks are bounded, addressable units used for (1) prompt assembly, (2) retrieval, (3) evidence pointers.

## Strategy by Kind

| Source | Chunk kind | Boundary rule (defaults, provisional) |
|---|---|---|
| Audio/Video transcript | `time_range` | Speaker-turn aware; target 45–90 s or ~400 tokens; never split mid-sentence |
| Document | `text_span` / `page_region` | Heading-aware sections, ~500 tokens, 10% overlap; tables as separate `table` chunks |
| Slides | `slide` | One chunk per slide (+ notes) |
| Image/handwriting | `page_region` | Layout blocks; one chunk per page when small |
| Video frames | `frame` | One per sampled frame caption |

## Locator

Each chunk stores a `locator` compatible with Evidence pointers (time range, page + bbox, text span offsets, slide, sheet/cell range) and `seq` for order.

## Chunk IDs in Prompts

Prompts receive chunk content wrapped with stable short handles (for example `[C12]`) and instructions that outputs must cite handles. The AI Gateway maps handles back to chunk IDs and rejects citations to handles not provided ([hallucination-controls](../ai/hallucination-controls.md)).

## Versioning

Chunking is part of the pipeline version of `normalized_content`; changing chunking rules bumps the version.
