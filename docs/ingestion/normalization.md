# Normalization

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §9 ("Results are converted into Normalized Content and Source Chunks") |

## Purpose

Defines **Normalized Content**: the source-type-independent structure all modules consume.

## Structure (`normalized_content` extraction payload)

```json
{
  "schema": "clerk.normalized-content@1",
  "source_id": "…",
  "source_kind": "audio",
  "languages": ["fa", "en"],
  "duration_ms": 3600000,
  "page_count": null,
  "blocks": [
    {
      "block_id": "b-0001",
      "type": "utterance",
      "text": "…",
      "language": "fa",
      "speaker_label": "S2",
      "locator": { "type": "time_range", "start_ms": 1272000, "end_ms": 1278000 },
      "confidence": 0.92,
      "origin_extraction_id": "…transcript"
    },
    {
      "block_id": "b-0102",
      "type": "handwritten_line",
      "text": "…",
      "locator": { "type": "bbox", "page": 2, "x": 0.1, "y": 0.42, "width": 0.7, "height": 0.04 },
      "confidence": 0.61,
      "alternatives": ["…", "…"]
    }
  ]
}
```

Block types: `heading`, `paragraph`, `list_item`, `table`, `table_cell`, `utterance`, `slide_title`, `slide_body`, `speaker_notes`, `caption`, `handwritten_line`, `image_region`, `frame`, `formula`.

## Rules

- Locators use the same pointer types as [Evidence](../knowledge/evidence-model.md) so evidence can be created directly from blocks.
- Text normalisation: Unicode NFC; Persian-specific normalisation (Arabic Yeh/Kaf → Persian forms, ZWNJ preservation, digit normalisation to canonical with original retained in `text_original` when changed); whitespace collapse; no semantic rewriting.
- Low-confidence OCR/ASR keeps `alternatives` for review.
- Offsets in text spans are Unicode code points in the normalised text.
- Normalized content is data, never instructions ([prompt-injection](../security/prompt-injection.md)).
