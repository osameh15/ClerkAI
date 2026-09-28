# AI Governance

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership + AI Engineer + Product |
| Blueprint | BP §16, R1 (NIST AI RMF + GenAI Profile), R2 (NIST SP 800-218A), R5 (OWASP LLM Top 10), R11 (EU AI Act timeline) |

## Objective

A documented process for AI risk, evaluation, rollout and failure response — drawing from NIST AI RMF (**Govern, Map, Measure, Manage**). Not a certification claim (BP §30).

## Mapping to NIST AI RMF Functions

| Function | Clerk practice | Documents |
|---|---|---|
| Govern | Roles and accountability (AI Engineer owns evaluation; Tech Lead owns gateway; Product owns acceptance); change records; this governance set | [ownership-raci](../execution/ownership-raci.md), [model-change-management](model-change-management.md) |
| Map | Per-module AI risk profile: intended use, users, failure impact, data categories, languages | module `ai-profile.md` files |
| Measure | Evaluation framework, golden datasets, calibration, production monitoring | [evaluation-strategy](../ai/evaluation/evaluation-strategy.md), [ai-observability](../ai/ai-observability.md) |
| Manage | Quality gates, human oversight, incident management, rollback, provider changes | [quality-gates](quality-gates.md), [human-oversight](human-oversight.md), [incident-management](incident-management.md) |

## AI Risk Register (initial)

| Risk | Impact | Controls |
|---|---|---|
| Wrong owner/deadline in official minutes | Business commitments misrecorded | Evidence, review items, approval, evaluation gates |
| Invented text in handwriting documents | Legal/document integrity | No silent invention rule, blocking review |
| Unsafe site observation severity underestimation | Safety | Severity always reviewable; conservative defaults; human approval |
| Persian quality gaps | Adoption, errors | Persian benchmarks, multi-provider routing, review |
| Prompt injection via uploaded documents | Data exfiltration/manipulation | Content isolation, no tools, output validation ([prompt-injection](../security/prompt-injection.md)) |
| Provider data use/leakage | Confidentiality | Vendor registry, no-train settings, tenant policy |
| Cost blow-up | Margin | Budgets, routing ceilings, anomaly alerts |
| Hallucinated study content | Education quality | Source-grounded mode, citation checks |

## Regulatory Watch

EU AI Act timeline (R11) for EU market planning; Clerk's launch modules are expected to be general-purpose documentation assistance (not high-risk categories), but transparency obligations are designed in ([transparency](transparency.md)). Re-assess per new module in its `ai-profile.md`.
