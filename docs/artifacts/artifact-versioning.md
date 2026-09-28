# Artifact Versioning

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain |
| Blueprint | BP §7.2 ("Artifacts are never overwritten"), §12, App. D ("Every edit creates a new version; AI/Human provenance is preserved") |

## Rules

1. `artifact_versions` is insert-only (trigger-enforced).
2. `version_no` increments by 1 per artifact; `parent_version_id` records lineage (linear in v1; branching reserved for future collaboration).
3. `artifacts.current_version_id` changes only via optimistic concurrency (`row_version` ↔ `ETag`).
4. Edits are submitted as full content or JSON Patch; the server applies the patch to the current version, validates, sanitises, and writes a new version. Patches touching bound fields may propagate to Knowledge ([editor-contract](editor-contract.md)).
5. **Coalescing:** rapid autosave edits by the same user within a short window (for example 60 s) may be coalesced into one version to avoid version explosion — the coalesced version replaces only the *pending, unreferenced* autosave draft of that user (implemented as a client-side debounce + server "draft buffer" per user that is not an ArtifactVersion until committed). Committed versions are never modified.
6. Section-level provenance (`section_origins`) is recomputed per version so the UI can distinguish AI-generated vs human-edited sections.
7. Diff between versions is computed on demand (structured JSON diff) for the version history UI.

## Retention

Versions follow the artifact's retention category; intermediate versions of final artifacts may be pruned by policy after finalisation (keeping v1 AI, all approved/final versions, and the last human version) — policy option, default keep all.
