# Implementation Order

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §0 ("We first build the platform that modules live on"), §29.3 ("Sprint 0 must not start with the Meeting Module"), App. F |

## Order

1. **Sprint 0 — Executable foundation** ([sprint-0](sprint-0.md)): repo, local stack, CI, identity/org/tenant context, RLS PoC, permission evaluator, module spec + validator + Hello Module, storage + upload-session skeleton, Temporal hello workflow, OTel, OpenAPI base.
2. **Sprint 1 — Platform vertical slice** ([sprint-1](sprint-1.md)): Hello Module installed from Store → job → upload → process (mock AI) → knowledge + evidence → artifact v1 → review → export prototype, with permission/entitlement/audit/usage.
3. **Implementation entry checklist** ([implementation-entry-checklist](implementation-entry-checklist.md)) must pass before real modules.
4. **Platform Core deepening (G5–G10)** in dependency order: module lifecycle completeness → universal ingestion per kind → Temporal job engine hardening → AI Gateway with real providers + evaluation runner → knowledge/evidence/review → artifact editor/templates/exports/approvals.
5. **Launch modules (G11)** in controlled parallel: recommended start with **Handwriting** (narrowest inputs; exercises review/evidence strongly) and **Meeting Minutes** (core B2B value), then **Site Report** (offline/PWA) and **Lecture Summary** (long audio + slides). Final order is a Product decision; all four must pass G11.
6. **Commerce (G12)** can proceed in parallel after G4 with a separate owner.
7. **Beta hardening (G13)** and **Launch (G14)**.

## Rule

No module-specific code is written before the Sprint 1 vertical slice proves the generic path.
