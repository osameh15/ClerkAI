# ADR-016: Artifact Content = Structured JSON + Renderer

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §12, §28.1 |

## Decision

- ArtifactVersion content is **schema-versioned JSON** conforming to the module's artifact schema and the platform envelope ([artifact.schema.json](../../api/schemas/artifact.schema.json)).
- Web, PDF and DOCX outputs are **renderings** produced from `ArtifactVersion + TemplateVersion + BrandKit` by platform renderers. Final HTML is never the source of truth.
- Rich text inside fields uses a constrained, sanitised inline-mark model (no raw HTML).
- Module code never implements PDF/DOCX layout.

## Consequences

Editor is a structured editor bound to schemas ([editor-contract](../../artifacts/editor-contract.md)); renderers need golden-file tests (BP App. D "Export fidelity").

## Related

[structured-content](../../artifacts/structured-content.md), [rendering](../../artifacts/rendering.md)
