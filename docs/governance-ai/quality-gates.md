# AI Quality Gates

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §11.4, §16.1, §24 (G8, G11), §29.2 |

## Gates

| Gate | When | Criteria | Doc |
|---|---|---|---|
| AI-PR | PR touching prompts/schemas/AI config | Smoke suite passes regression gates | [regression-gates](../ai/evaluation/regression-gates.md) |
| AI-RC | Release candidate with AI changes | Full affected suites pass; cost within tolerance | same |
| AI-MODEL | Profile route/model change | Change record + full suites + canary health | [model-change-management](model-change-management.md) |
| G8 exit | AI Platform | Provider swap requires no module change; ≥ 2 adapters (or 1 + mock); schema validity ≥ 99.5% on benchmark | [execution-roadmap](../execution/execution-roadmap.md) |
| G11 exit | Four modules | Module golden suites meet thresholds; acceptance criteria demonstrated | module `acceptance-criteria.md` |
| Beta monitoring | G13 | Correction rate, acceptance rate, evidence coverage tracked per module weekly | [ai-observability](../ai/ai-observability.md) |

## Exceptions

A gate exception requires written approval by Tech Lead + Product with expiry date and remediation plan, recorded in the [risk-register](../execution/risk-register.md). Hallucination and tenant-safety gates have no exception path.
