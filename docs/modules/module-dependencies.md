# Module Dependencies

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4 ("dependency resolution"), App. E (`module_dependencies`) |

## Declaration

```yaml
dependencies:
  - module: business.meeting-minutes
    version: "^1.0.0"
    optional: true
    reason: Reuses the minutes artifact schema and template.
```

## What a Dependency Allows

A dependent module may reference the dependency's **published** artifact schemas, templates and knowledge subtypes by versioned reference (for example `business.meeting-minutes/minutes@1.0.0`). It may not call the dependency's native code or share runtime state.

## Resolution Rules

- Required dependency missing → installer installs it (if entitled) in the same operation, else fails with `MODULE_DEPENDENCY_UNSATISFIED`.
- Optional dependency missing → module installs; features depending on it are hidden (`ui` fallback) — for example Handwriting's "Meeting Minutes" output uses its own bundled schema copy if Meeting Minutes is not installed (declared in its manifest artifacts).
- Version ranges resolved to the installed version of the dependency; conflicting ranges across installed modules → install blocked with explanation.
- Cycles are rejected at validation.
- Uninstalling a module that others require → blocked unless dependents are uninstalled first; disabling is allowed.
