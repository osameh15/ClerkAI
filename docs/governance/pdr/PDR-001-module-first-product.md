# PDR-001: Clerk Is a Modular Platform, Not a Construction-Only App

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Product / Founder |
| Blueprint | BP §0, §1.1, §28.2 |

## Context

The strongest early use case (Site Report) could pull the product into a vertical construction tool. The strategic differentiation is the reusable pipeline Sources → Knowledge → Evidence → Artifact → Workflow across many domains.

## Decision

Clerk is positioned and built as a modular AI documentation platform. Every domain capability ships as a Module installed through the Module Store. Platform Core contains no domain-specific behaviour.

## Consequences

- Product roadmap items are phrased as "platform capability" or "module"; features needed by one module are first evaluated as generic capabilities.
- Future modules (Inspection Report, HSE, NCR, Daily Construction Report, Interview Summary, Business Letter, Board Minutes, Study Notes, Exam Builder, …) must be possible without Core rewrites. This is the platform's architectural success test.

## Related

[AP-01](../architecture-principles.md#ap-01-module-first), [module-platform](../../modules/module-platform.md)
