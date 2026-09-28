# Module Authoring Guide

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4, §1.1 (Module-first) |

## Audience

Engineers creating modules (official now; partners later).

## Steps

1. **Define the outcome:** artifacts the user needs; write the module product spec (use a launch module's `product-spec.md` as template).
2. **Choose key and category:** `<category>.<name>`; immutable.
3. **Model inputs:** slots by source kind, required extractions, capture modes, offline needs, readiness rule.
4. **Model knowledge:** entity subtypes over platform base types; relations; which fields need evidence; review thresholds; high-risk types.
5. **Design artifacts:** schemas using platform `$defs`; generation mode (mapping/AI/hybrid); templates; approval requirement; numbering; action item creation.
6. **Write AI tasks:** capability + profile (never a model), prompt with isolation rules, output schema with citations.
7. **Compose the processing workflow** from platform step types ([workflow-reference](workflow-reference.md)); avoid native steps. If a native step seems needed, first propose a new generic platform step type.
8. **Declare permissions and entitlements.**
9. **Choose UI mode:** start `generic`; use `hybrid` extensions only for capture/review experiences that schemas cannot express.
10. **Build evaluation:** annotation guidelines, golden dataset plan, metrics and thresholds.
11. **Test** ([testing-modules](testing-modules.md)) and **publish** ([publishing-modules](publishing-modules.md)).

## Anti-Patterns (rejected in review)

- Asking Core to add a module-specific table, endpoint or `if module == …`.
- Referencing a provider/model name.
- Artifact fields holding HTML.
- Evidence-required facts without citations in AI output schemas.
- Guessing unreadable/ambiguous values instead of producing review items.
