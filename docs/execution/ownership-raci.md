# Ownership and RACI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Founder + Tech Lead |
| Blueprint | BP §26, §26.1 |

## Roles (BP §26)

| Role | Responsibility |
|---|---|
| Product / Founder | Scope, customer discovery, pricing, acceptance |
| Tech Lead / Go | Architecture, domain, API, Temporal, security |
| Frontend / Nuxt | PWA, design system, editor, module UI |
| AI Engineer | Providers, prompts, evals, multimodal pipelines, dataset |
| Product Designer | Flows, cards/store/editor, mobile site UX |
| QA / Security | Automation, security review, release gates |
| DevOps / SRE (shared) | CI/CD, observability, backups, infrastructure |

One person may hold several roles; responsibilities stay explicit.

## RACI (R = Responsible, A = Accountable, C = Consulted, I = Informed)

| Area | Founder | Tech Lead | Frontend | AI Eng | Designer | QA/Sec | DevOps |
|---|---|---|---|---|---|---|---|
| Product scope, PDRs | A/R | C | C | C | C | I | I |
| Pricing, BDRs | A/R | C | I | C | I | I | I |
| Architecture, ADRs | C | A/R | C | C | I | C | C |
| Data model, migrations | I | A/R | I | C | I | C | C |
| Tenancy & authorization | I | A/R | C | I | I | R | I |
| API contract (OpenAPI) | I | A/R | R | C | I | C | I |
| Module platform | C | A/R | R | C | C | C | I |
| Ingestion pipelines | I | A | I | R | I | C | C |
| AI Gateway & registries | I | A | I | R | I | C | C |
| Evaluation & datasets | C | C | I | A/R | I | C | I |
| Knowledge & evidence | I | A/R | C | R | C | C | I |
| Artifacts, templates, exports | C | A/R | R | C | R | C | I |
| Frontend / PWA / design system | C | C | A/R | I | R | C | I |
| Launch module specs & acceptance | A | R | R | R | R | R | I |
| Security controls & gates | I | A | C | C | I | R | R |
| Privacy & retention | A | R | C | C | I | R | C |
| CI/CD, environments, observability | I | A | C | C | I | C | R |
| Incident response | I | A | C | C | I | R | R |
| Billing implementation | A | R | R | I | C | C | I |
| Documentation governance | C | A | C | C | C | C | C |
