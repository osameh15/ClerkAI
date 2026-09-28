# Construction Site Report (`construction.site-report`)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 (module v1.0.0) |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer (module), Frontend (mobile/offline) |
| Blueprint | BP §3, §3.2, §21, App. A (Site Report Flow), App. D, EPIC-17, [PDR-007](../../governance/pdr/PDR-007-site-report-mobile-offline.md) |

## Summary

Documents a construction or field visit with minimal friction on mobile, including **offline capture**. Produces Observations with location, category, severity, issue, corrective action, responsible party, deadline and evidence; supports **Repeat Inspection** with before/after evidence; tracks corrective actions as independent Action Items (Open / Resolved / Overdue); routes a formal Site Report through approval.

## Specification Set

[product-spec](product-spec.md) · [inputs](inputs.md) · [knowledge-schema](knowledge-schema.md) · [artifact-schema](artifact-schema.md) · [workflow](workflow.md) · [permissions](permissions.md) · [configuration](configuration.md) · [offline-behavior](offline-behavior.md) · [repeat-inspection](repeat-inspection.md) · [ai-profile](ai-profile.md) · [evaluation-plan](evaluation-plan.md) · [acceptance-criteria](acceptance-criteria.md) · [edge-cases](edge-cases.md) · Manifest: [site-report.example.yaml](../manifests/site-report.example.yaml)

## Designed for Unreliable Networks

Capture, local queueing and sync are first-class ([offline-behavior](offline-behavior.md), [pwa/offline-site-report](../../pwa/offline-site-report.md)).
