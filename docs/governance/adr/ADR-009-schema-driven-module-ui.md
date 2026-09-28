# ADR-009: Module UI = Schema-Driven + Optional Custom/Hybrid

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership, Frontend Lead |
| Blueprint | BP §8, §20, §28.1 |

## Context

The frontend must discover Modules from the API, not hard-code cards, and must not know module processing internals. Some modules (Site Report capture, Lecture flashcards) need specialised UI.

## Decision

A Module declares `ui.mode`:

| Mode | Behaviour |
|---|---|
| `generic` | All forms (configuration, job input, review, artifact editing) rendered from JSON Schemas + UI hints. |
| `hybrid` | Generic rendering plus named **extension points** filled by registered frontend components shipped with the platform (for example `capture.mobile-observation`, `artifact.flashcards-viewer`). |
| `custom` | A full custom surface for a flow. v1: only official modules; the component must still use the Web SDK and platform APIs. |

Frontend components for `hybrid`/`custom` are part of the Nuxt application and registered by key; the manifest only references keys. There is no remote JavaScript loading in v1.

## Consequences

- Platform must ship a schema-form renderer supporting the [UI hint vocabulary](../../modules/module-ui-contract.md).
- Launch modules may use `hybrid` (BP guidance).

## Related

[module-ui-contract](../../modules/module-ui-contract.md), [frontend-architecture](../../frontend/frontend-architecture.md)
