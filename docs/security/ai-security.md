# AI Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Security |
| Blueprint | BP §14.3, R2 (NIST SP 800-218A), R5 (OWASP LLM Top 10 2025) |

## BP §14.3 Requirements

| Requirement | Control |
|---|---|
| Source content is never trusted instruction | Delimited untrusted content; instruction hierarchy in prompts; no tools that act on instructions ([prompt-injection](prompt-injection.md)) |
| Tool calling minimal and allowlisted; no arbitrary network actions | v1: **no model tool-calling** for business tasks; gateway uses structured output only |
| Structured output validated and sanitised | [output-validation](output-validation.md) |
| Renderers never execute model HTML/URLs | Structured content only; sanitised links |
| RAG tenant-scoped and permission-aware | [retrieval](../knowledge/retrieval.md) |
| No secrets or system prompts exposed | No secrets in prompts; provider errors mapped; outputs validated against schema (system prompt text cannot appear in schema fields unnoticed; canary strings detection) |
| Low-confidence/high-risk → human review | [human-oversight](../governance-ai/human-oversight.md) |

## OWASP LLM Top 10 (2025) Mapping

| Risk | Clerk control |
|---|---|
| LLM01 Prompt Injection | Isolation, schema outputs, citation checks, no tools, human review |
| LLM02 Sensitive Information Disclosure | Tenant-scoped context, redaction policy, provider no-train, payload retention limits |
| LLM03 Supply Chain | Vendor registry, pinned model versions, evaluation before change |
| LLM04 Data and Model Poisoning | No training on customer data by default; curated, reviewed datasets |
| LLM05 Improper Output Handling | Validation, sanitisation, no HTML |
| LLM06 Excessive Agency | No autonomous actions; no outbound actions triggered by models |
| LLM07 System Prompt Leakage | No secrets in prompts; leakage detection canaries |
| LLM08 Vector and Embedding Weaknesses | Tenant filters before ranking; embeddings under RLS |
| LLM09 Misinformation | Evidence-first, hallucination controls, review |
| LLM10 Unbounded Consumption | Budgets, rate limits, cost ceilings |

## Provider Privacy Configuration

Use provider options for zero data retention / no training where available; regional endpoints per tenant policy; provider credentials isolated per provider; tenant identifiers sent as opaque hashes.
