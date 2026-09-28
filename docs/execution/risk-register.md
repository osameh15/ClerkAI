# Risk Register

| Field | Value |
|---|---|
| Status | Accepted (living) |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §27 |

## Blueprint Risks (BP §27)

| ID | Risk | Severity | Mitigation (BP) | Clerk documentation controls | Owner | Status |
|---|---|---|---|---|---|---|
| R-01 | Over-engineering before market validation | Medium/High | Fully design Core contracts; don't feature-complete Enterprise/Marketplace until demand | [v1-scope](../product/v1-scope.md) separates architecture-ready vs implemented | Tech Lead | Open |
| R-02 | Persian AI quality | High | Persian benchmark, multiple providers, human review, dataset flywheel | Golden datasets with Persian majority; OD-015 decided by benchmark; review-first UX | AI Engineer | Open |
| R-03 | AI cost | High | Metering, routing, dedup, quotas, margin dashboard | [quotas](../billing/quotas.md), [cost-monitoring](../operations/cost-monitoring.md), cost regression gates | Founder | Open |
| R-04 | Module Store complexity | Medium | Declarative-first; third-party runtime later | [ADR-008](../governance/adr/ADR-008-declarative-first-modules.md), M0 only | Tech Lead | Open |
| R-05 | Data sensitivity | High | Privacy/security by design, audit, retention, vendor controls | security/ and privacy/ sets | Security | Open |
| R-06 | DOCX/PDF fidelity | Medium | Structured artifacts + renderer tests + constrained templates | Golden export tests; OD-008/009 bake-off | Tech Lead | Open |
| R-07 | Offline complexity | Medium | Limited queue + explicit status in Site MVP; evolve gradually | [offline-first](../pwa/offline-first.md) staged delivery | Frontend Lead | Open |
| R-08 | Scope creep | High | Four fixed launch modules; PDRs and gates | [PDR-003](../governance/pdr/PDR-003-four-launch-modules.md) | Product | Open |
| R-09 | Vendor lock-in | Medium | AI/provider abstraction + S3/OpenTelemetry standards | ADR-017, ADR-015, ADR-014 | Tech Lead | Open |
| R-10 | Early Enterprise demands | Medium | SSO/data-residency architecture-ready; implement on signal | [enterprise-data-controls](../privacy/enterprise-data-controls.md) | Founder | Open |

## Risks Identified During Documentation

| ID | Risk | Severity | Mitigation | Owner | Status |
|---|---|---|---|---|---|
| R-11 | Workflow DSL (OD-018) too weak → pressure for native steps, eroding declarative-first | High | Prototype DSL with Hello + one real module flow before G5 exit; extend step library generically | Tech Lead | Open |
| R-12 | Temporal non-determinism during rapid iteration | Medium | Replay tests in CI from Sprint 0 | Tech Lead | Open |
| R-13 | RLS performance/complexity with pooled connections | Medium | `SET LOCAL` pattern + load tests; EXPLAIN reviews | Tech Lead | Open |
| R-14 | Provider availability/sanctions in target markets (Iran) | High | Multi-provider routing, local/open-weight options, vendor registry; hosting decision OD-005 | Founder | Open |
| R-15 | Payment provider availability by region | High | PaymentProvider port; manual invoicing adapter; OD-010 | Founder | Open |
| R-16 | Evidence precision (time/bbox alignment) insufficient for user trust | Medium | Evidence alignment metrics; chunk-level fallback; UX shows context | AI Engineer | Open |
| R-17 | Confidence miscalibration causes silent errors | Medium | Calibration metrics; conservative thresholds; blocking review for high-risk types | AI Engineer | Open |
| R-18 | Small team vs broad scope (3–5 people) | High | Gate-based sequencing; strict v1-scope; defer Marketplace/Enterprise | Founder | Open |
| R-19 | Offline data exposure on shared/lost devices | Medium | Purge after sync; OD-023 decision; sign-out safeguards | Security | Open |
| R-20 | Legal requirements for meeting recording consent vary | Medium | Configurable notice; OD-032 legal review | Product | Open |
| R-21 | FA/EN Blueprint wording divergence unnoticed | Low | Structure verified identical; EN authoritative; review FA on next Blueprint revision | Documentation Lead | Open |

## Existing Implementation Conflicts

None — the repository contained only the Blueprint source documents when this documentation was created (no code to reconcile).
