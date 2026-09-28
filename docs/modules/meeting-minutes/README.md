# Meeting Minutes (`business.meeting-minutes`)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 (module v1.0.0) |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer (module), Module Platform (contract) |
| Blueprint | BP §3, §3.1, App. A (Meeting Minutes Flow), App. D, EPIC-16 |

## Summary

Transforms a live or uploaded meeting (audio/video), agenda, PDFs, images and handwritten notes into **formal meeting minutes and reusable Knowledge**. The primary outputs are structured Decisions, Actions, Owners, Deadlines, Risks and Open Questions with evidence and confidence; the transcript is secondary ([PDR-005](../../governance/pdr/PDR-005-transcript-not-primary.md)).

## Specification Set

| Doc | Content |
|---|---|
| [product-spec](product-spec.md) | Goals, users, scope, distinctions, flows |
| [inputs](inputs.md) | Slots, formats, limits, recording |
| [knowledge-schema](knowledge-schema.md) | Entity subtypes, attributes, relations, evidence |
| [artifact-schema](artifact-schema.md) | Minutes, action list, executive summary, follow-up, transcript |
| [workflow](workflow.md) | Processing DAG, review, approval |
| [permissions](permissions.md) | Module permissions |
| [configuration](configuration.md) | Configuration schema |
| [ai-profile](ai-profile.md) | AI tasks, profiles, prompts, AI risk profile |
| [evaluation-plan](evaluation-plan.md) | Datasets, metrics, gates |
| [acceptance-criteria](acceptance-criteria.md) | Testable criteria |
| [edge-cases](edge-cases.md) | Edge cases and expected behaviour |
| Manifest | [meeting-minutes.example.yaml](../manifests/meeting-minutes.example.yaml) |

## Platform Dependencies

Ingestion (audio/video/document/handwriting), AI Gateway, Knowledge (fusion, evidence, review), Artifacts (templates, numbering, approval), Actions. No Core changes required.
