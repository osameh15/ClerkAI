# ADR-017: Provider-Neutral AI Gateway with Capability Profiles

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership, AI Engineer |
| Blueprint | BP §1.1 "Vendor-neutral AI", §11.1, §11.2, G8 exit criterion |
| Note | Formalises a Blueprint principle; not listed in BP §28.1. Proposed as ADR-013 in the documentation brief; renumbered. |

## Decision

- Modules request **capabilities** via Model Profiles (for example `capability: structured_extraction, quality: high, language: fa, modalities: [audio, image], schema: meeting_actions_v2`). They never name providers or models.
- The **AI Gateway** routes using quality, language, modality, cost, latency, availability and tenant AI policy; handles fallback; validates structured output; records an AIExecution per attempt.
- Provider adapters implement a single `Provider` port. Provider credentials are isolated per provider and never exposed to module definitions.
- G8 exit: swapping the provider behind a profile requires **no Module change** (BP §24).

## Consequences

Model Registry and Prompt Registry are platform data; prompts may declare compatible profiles; evaluation runs per (prompt version × model) pair.

## Related

[ai-gateway](../../ai/ai-gateway.md), [model-routing](../../ai/model-routing.md), [provider-contract](../../ai/provider-contract.md)
