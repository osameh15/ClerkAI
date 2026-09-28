# Module Validation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §18.1 ("module validation" CI step), §19 (Contract: Module manifests), App. F (manifest validator) |

## Validation Levels

| Level | Checks |
|---|---|
| L1 Manifest schema | Validates against [module-manifest.schema.json](../api/schemas/module-manifest.schema.json); YAML parsed with a **YAML 1.2 / strict** loader (YAML 1.1 turns keys like `on`, `yes`, `no` into booleans) |
| L2 References | Every referenced path exists in the package; every referenced JSON Schema compiles (2020-12); prompt refs resolve; template keys unique; dataset refs exist |
| L3 Namespaces | Permissions under module key namespace; entity subtype keys prefixed consistently; relation keys uppercase; artifact types unique |
| L4 Semantics | Readiness slots exist; `requiredExtractions` supported for slot kinds; `ai.tasks[].profile` exists in model profile catalogue; `capabilities` known; workflow DAG acyclic and step params valid per step type; entity types referenced in workflows/artifacts exist; `createsActionItems` types are `action`-based |
| L5 Runtime policy | `native` only for `clerk-official`; `remote` rejected in v1; UI extension components exist in current frontend registry (warning if not, fallback required) |
| L6 Compatibility | `compatibility.platform` satisfiable by current platform; dependency ranges resolvable; upgrade: breaking changes require MAJOR bump (schema diff rules) |
| L7 Security | No absolute paths/`..`; no scripts in templates; size limits; translation strings sanitised |
| L8 Evaluation | Suites declared for every AI task with `releaseGate`; runs green before publication |

## Tooling

`module-spec validate <path>` (CI step for all modules); the registry runs the same validator server-side at publication. Launch module manifests in this documentation validate at L1 in CI ([testing-modules](sdk/testing-modules.md)).
