# Structured Content

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain |
| Blueprint | BP §12 ("Artifact content is stored as schema-versioned JSON… Final HTML is not the source of truth") |

## Envelope

Every ArtifactVersion `content` conforms to [artifact.schema.json](../api/schemas/artifact.schema.json):

| Field | Purpose |
|---|---|
| `schema_ref` | Module artifact schema version (`business.meeting-minutes/minutes@1.0.0`) |
| `language`, `direction` | Rendering and editor direction (RTL for Persian) |
| `title`, `metadata` | Document-level fields for templates |
| `body` | Module-specific structured content |
| `bindings[]` | Content path → Knowledge entity (+ attribute) |
| `section_origins[]` | AI/human/system per section |
| `warnings[]` | Unresolved review items, low confidence, missing evidence per path |

## Building Blocks (`$defs` in the envelope schema)

`richText` (text runs with limited marks and https links, mentions of Knowledge entities), `paragraphs`, `table`, `personRef`, `dateValue` (date + original text + calendar), `imageRef` (source file + evidence link). Module schemas compose these; they must not introduce raw HTML fields.

## Validation Pipeline

1. JSON Schema validation (envelope, then module schema via `schema_ref`).
2. Sanitisation: strip control characters, normalise Unicode, reject disallowed link schemes, cap sizes.
3. Binding integrity: bound entity IDs exist in the job's Knowledge.
4. Warning recomputation.

## Why Not HTML

HTML as source of truth prevents validation, safe rendering, multi-format output and knowledge binding. Renderers produce HTML/PDF/DOCX from JSON + template ([rendering](rendering.md)).
