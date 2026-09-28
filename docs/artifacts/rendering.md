# Rendering

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (engines Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain |
| Blueprint | BP §12 ("Export: Async rendering; PDF/DOCX initially, Email/Slides later"), §14.3 ("Artifact renderers never execute raw HTML or URLs produced by the model"), §27 (DOCX/PDF fidelity) |

## Renderer Service

- Runs on the `export` worker pool in a sandbox without network egress; receives only: sanitised content envelope, template version, brand kit version, fonts, and pre-fetched images (renditions resolved by the orchestrator).
- `Renderer` port per format: `web` (HTML for in-app view, generated server- or client-side from the same template semantics), `pdf`, `docx`; future `email`, `slides`.
- Export flow: `POST /v1/artifacts/{id}/exports` → `ArtifactExportWorkflow` → render → upload to exports bucket → `artifact_exports.succeeded` → event → notification; download via presigned URL (`GET /v1/exports/{id}`).

## Fidelity Controls (BP §27)

- Structured artifacts + constrained templates reduce layout variability.
- Golden-file tests: fixed inputs → rendered outputs compared (PDF: text extraction + page count + visual diff threshold; DOCX: XML structure assertions) (BP App. D "Golden PDF/DOCX samples render without clipping or broken layout").
- Persian shaping/bidi tested with golden Persian and mixed-direction samples.

## Security

No model-provided HTML or URLs executed; links rendered as text + https hyperlink only if in allowlisted scheme; images only from platform renditions; renderer has no credentials except write to the single output key (presigned).

## Metering

`artifact.export` meter (count/pages).

## Engines

PDF [OD-008](../governance/open-decisions.md#od-008), DOCX [OD-009](../governance/open-decisions.md#od-009). Details: [pdf-export](pdf-export.md), [docx-export](docx-export.md).
