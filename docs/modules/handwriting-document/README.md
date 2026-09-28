# Handwriting → Document (`general.handwriting-document`)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 (module v1.0.0) |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |
| Blueprint | BP §3, §3.3, App. A (Handwriting Flow), App. D, EPIC-18 |

## Summary

Converts one or more photos/scans/PDF pages of handwritten notes into clean, reviewable documents. Extracts text and structure, highlights low-confidence spans for mandatory review, and proposes multiple possible artifacts from the **same Source**: Clean Document, Formal Letter, Meeting Minutes, Task List ([PDR-004](../../governance/pdr/PDR-004-source-reusability.md)).

## Specification Set

[product-spec](product-spec.md) · [inputs](inputs.md) · [knowledge-schema](knowledge-schema.md) · [artifact-schema](artifact-schema.md) · [workflow](workflow.md) · [permissions](permissions.md) · [configuration](configuration.md) · [ai-profile](ai-profile.md) · [evaluation-plan](evaluation-plan.md) · [acceptance-criteria](acceptance-criteria.md) · [edge-cases](edge-cases.md) · Manifest: [handwriting-document.example.yaml](../manifests/handwriting-document.example.yaml)
