# Document Processing (PDF, DOCX)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §9 (PDF: text/layout → tables/images → page references; DOCX: native parse → text/table/image → structure) |

## PDF

| Step | Output |
|---|---|
| Parse (sandboxed) | page count, text layer presence per page, metadata |
| Text layer extraction | `text` with page + character offsets |
| Scanned page detection | pages without meaningful text layer → page image rendition → `ocr` / `handwriting_ocr` |
| Layout | `layout` blocks per page with boxes, reading order |
| Tables | `tables` (cells with page/bbox) |
| Embedded images | extracted as renditions; optional `vision_caption` on demand |
| Page references | every chunk carries `page` (+ bbox/text span) |

## DOCX

Native parse of the OOXML package: headings (styles), paragraphs, lists, tables, embedded images, comments (ignored by default), tracked changes (accepted view). Page numbers are not intrinsic in DOCX; locators use paragraph index + character offsets (`text_span`) and headings path.

## RTL and Persian

Correct logical order extraction for RTL text in PDFs (visual-order PDFs are re-ordered using bidi analysis); Persian normalisation per [normalization](normalization.md).

## Security

No macro execution, no external resources, zip-bomb checks ([mime-validation](mime-validation.md)); parsers run in the isolated document worker with CPU/memory/time limits.
