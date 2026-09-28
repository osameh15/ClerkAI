# ADR-008: Module Runtime = Declarative-First

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §4, §4.2, §1.2, §23, §28.1 |

## Context

Modules must be addable without changing Core, and a future Marketplace must not execute arbitrary third-party code inside the main process.

## Decision

Three runtime types exist in the contract; only the first two execute in v1:

| Type | Definition | v1 |
|---|---|---|
| `declarative` | Manifest + schemas + workflow definition + prompts + templates, interpreted by platform-provided generic workflows and step types. | **Preferred; default for all modules.** |
| `native` | Official Go handler compiled into Clerk and registered against a step type. Only for trusted/official modules and only where declarative expression is insufficient. | Allowed for `clerk-official` publisher only, per-step, with review. |
| `remote` | External execution via a signed API contract through a Remote Module Gateway with scoped permissions; no direct DB access. | Future (Marketplace M2). Contract reserved. |

- Loading Go plugins (`.so`) or any dynamic third-party code into the API/worker process is **forbidden**.
- Native handlers implement the same step interfaces declarative steps use; they cannot bypass Knowledge/Artifact/Audit APIs.

## Consequences

- The platform must provide a sufficiently expressive step library ([workflow-reference](../../modules/sdk/workflow-reference.md), [OD-018](../open-decisions.md#od-018)).
- Launch modules should need little or no native code; any native step is recorded in the module's README with justification.

## Related

[module-runtime-contract](../../modules/module-runtime-contract.md), [module-sandboxing](../../security/module-sandboxing.md), [remote-modules](../../marketplace/remote-modules.md)
