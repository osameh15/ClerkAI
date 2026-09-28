# Product Principles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §1, §1.2, §2, §20, §28.2 |

## Purpose

Product-level rules that constrain scope and UX decisions. Architecture principles live in [architecture-principles.md](architecture-principles.md).

## Positioning

"From real-world input to verified business artifact." Clerk behaves like an *intelligent documentation operating system*: the user picks the output they need from Module cards, adds one or more Sources, and Clerk analyses, fuses and validates the material before producing the output in the organization's template.

The protected value chain:

```text
Unstructured Sources → Understanding → Structured Knowledge → Evidence → Professional Artifact → Workflow
```

## Principles

| ID | Principle | Consequence |
|---|---|---|
| PP-01 | **Output-first, not model-first.** Users choose outputs (cards), never models or prompts. | No model picker in the product UI; AI policy is an admin setting. |
| PP-02 | **Modular platform, not a vertical app** ([PDR-001](pdr/PDR-001-module-first-product.md)). | Every launch feature is delivered as a Module on generic platform capabilities. |
| PP-03 | **Card launcher home and a first-class Module Store** ([PDR-002](pdr/PDR-002-card-launcher-and-module-store.md)). | Home cards are API-driven from installed modules. |
| PP-04 | **Two entry modes**: Quick Clerk (source-first) and Module-first. | Sources can be captured before a module is chosen (Inbox). |
| PP-05 | **Sources are reusable** ([PDR-004](pdr/PDR-004-source-reusability.md)). | One upload can serve multiple Jobs and Artifacts. |
| PP-06 | **Structure over transcripts** ([PDR-005](pdr/PDR-005-transcript-not-primary.md)). | Transcripts are secondary outputs; Decisions/Actions/etc. are primary. |
| PP-07 | **Review and Evidence are core value** ([PDR-006](pdr/PDR-006-human-review-and-evidence.md)). | Every important fact can be opened at its source; uncertain items go to a review queue. |
| PP-08 | **Draft until confirmed.** AI output is always presented as Draft until confirmation/approval. | Status labels and disclosure in the editor. |
| PP-09 | **Mobile and unreliable networks are real** ([PDR-007](pdr/PDR-007-site-report-mobile-offline.md)). | Capture works offline for Site Report; sync status is explicit. |
| PP-10 | **Persian first-class.** Persian and mixed Persian/English content, RTL layout, Jalali dates are supported from v1. | Evaluation datasets include Persian; UI supports RTL. |
| PP-11 | **No silent invention.** Unreadable or unsupported values never enter an official document as guesses. | Low-confidence fields block finalisation until resolved. |
| PP-12 | **Every state is designed**: empty, loading, error, permission-denied, entitlement-upgrade, offline. | Definition of Done includes these states ([definition-of-done](../execution/definition-of-done.md)). |

## What Clerk Is Not (BP §1.2)

Clerk is **not**: a general-purpose ERP; a general-purpose CRM; a full project-management suite (Jira, ClickUp); merely voice-to-text or OCR; simply a chat interface on top of an LLM; and in v1 not a platform that executes arbitrary third-party code inside the main process. See [non-goals.md](../product/non-goals.md) for how these translate into rejected features.

## Scope Control

- Four fixed launch modules ([PDR-003](pdr/PDR-003-four-launch-modules.md)).
- New features enter through PDRs and Gates ([execution-roadmap](../execution/execution-roadmap.md)).
- Action Items exist to track commitments extracted from artifacts, not to become a task-management suite.

## Related Documents

- [product-overview.md](../product/product-overview.md), [product-boundaries.md](../product/product-boundaries.md), [v1-scope.md](../product/v1-scope.md)
