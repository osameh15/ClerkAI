# Product KPIs

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (definitions); Provisional (targets) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §2.1 |

## Purpose

Defines each KPI precisely enough to compute from platform data, and names the data source. Targets are not set until beta data exists.

## KPI Definitions

| KPI | Definition (BP §2.1) | Computation | Data sources |
|---|---|---|---|
| **Time-to-Artifact** (North Star candidate) | Time from Job submission to reviewable draft | `first artifact_versions.created_at (origin=ai)` − `jobs.submitted_at`, p50/p90 per module | `jobs`, `artifact_versions` |
| Artifact Acceptance Rate | % outputs accepted with minimal human correction | Artifacts reaching `approved`/`final` whose human edit distance from the AI version ≤ threshold (TBD, [OD-022](../governance/open-decisions.md#od-022)) ÷ artifacts reaching `approved`/`final` | `artifact_versions` (origin, diff metrics) |
| Human Correction Rate | Volume of human change to AI-generated sections; should trend down | Σ `human_modified` + `rejected` review outcomes ÷ AI-generated knowledge items; plus normalised edit distance per artifact | `knowledge_entities.review_state`, `artifact_versions` |
| Evidence Coverage | % important facts with valid, accessible evidence | Knowledge items of evidence-required types with ≥1 non-superseded, resolvable evidence link ÷ all such items | `knowledge_entities`, `evidence_links` |
| Module Activation | % installations leading to a successful first Job | Installations with ≥1 `completed` Job within 14 days ÷ installations | `module_installations`, `jobs` |
| Weekly Active Organizations | Orgs actively using Clerk weekly | Distinct `organization_id` with ≥1 submitted Job in ISO week | `jobs` |
| Expansion MRR | Incremental revenue from Module Packs, Usage, Seats | Billing provider data aggregated by line-item type | billing context |
| Gross Margin after AI | Margin after model, storage, processing cost | (Revenue − Σ attributed cost) ÷ Revenue per org/plan/module | `usage_events`, invoices ([unit-economics](../billing/unit-economics.md)) |

## Instrumentation Requirements

- `jobs.submitted_at`, `completed_at`; ArtifactVersion `origin` (`ai`, `human`, `system`) and `parent_version_id` are mandatory fields.
- Product analytics events are derived from domain events, not from ad-hoc client tracking ([event-architecture](../architecture/event-architecture.md)).
- KPIs exclude internal/test organizations (`organizations.is_internal`).

## Related Documents

[unit-economics](../billing/unit-economics.md), [metrics](../operations/metrics.md)
