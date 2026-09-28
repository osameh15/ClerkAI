# Editor Contract

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (editor library Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain + Frontend |
| Blueprint | BP §20.1 (Universal Artifact Editor), App. A (Artifact Editor), App. B (`PATCH /v1/artifacts/{id}` optimistic concurrency) |

## Contract Between Backend and Editor

| Aspect | Contract |
|---|---|
| Input | `GET /v1/artifacts/{id}` → envelope + module artifact JSON Schema + UI schema (from module package) + `ETag` |
| Rendering in editor | Schema-driven sections; module `ui.extensions` may replace a section renderer (`artifact.section`) |
| Rich text | Constrained `richText` model (no HTML); editor library ([OD-014](../governance/open-decisions.md#od-014)) must serialise to/from it losslessly |
| Save | `PATCH` with `If-Match` + JSON Patch ops or full content; debounce autosave (default 2 s) |
| Conflict | `412 ARTIFACT_VERSION_CONFLICT` → editor fetches latest, shows three-way merge for structured fields where possible, else "reload with your changes kept aside" |
| Knowledge propagation | For bound fields, `apply_to_knowledge=true` (default) records a Knowledge correction (`human_modified`); free-text sections don't |
| Evidence | Bound fields show evidence badges; clicking opens Evidence Viewer ([evidence-display](evidence-display.md)) |
| Warnings | `warnings[]` shown inline; resolving the linked review item clears them after regeneration |
| Read-only states | `in_review` (default), `approved`, `final`; server enforces |
| Direction | RTL/LTR per `direction`, mixed-direction runs supported |

## Three-Panel Layout (BP §20.1)

Sources panel (audio, image, PDF with synchronised seek) · Artifact editor (structured sections, editable fields/tables, versioned content) · AI/Review panel (warnings, evidence, confidence). Details: [artifact-editor](../frontend/artifact-editor.md).
