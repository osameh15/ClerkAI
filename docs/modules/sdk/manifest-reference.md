# Manifest Reference

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 (manifestVersion "1.0") |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4 |

Normative contract: [module-manifest.schema.json](../../api/schemas/module-manifest.schema.json). This reference explains each section. Examples: [meeting-minutes](../manifests/meeting-minutes.example.yaml), [site-report](../manifests/site-report.example.yaml), [handwriting-document](../manifests/handwriting-document.example.yaml), [lecture-summary](../manifests/lecture-summary.example.yaml).

## Top Level

| Key | Req | Description |
|---|---|---|
| `manifestVersion` | ✓ | `"1.0"` (string) |
| `module` | ✓ | Identity |
| `runtime` | ✓ | `declarative` / `native` / `remote` |
| `compatibility` | ✓ | Platform contract range |
| `dependencies` | | Other modules |
| `capabilities` | | Required platform capabilities |
| `inputs` | ✓ | Source slots, limits, readiness |
| `configuration` | | Org/team/project settings schema |
| `jobInput` | | Per-job input schema |
| `knowledge` | ✓ | Entity subtypes, relations |
| `artifacts` | ✓ | Artifact types, schemas, templates, approval, numbering |
| `workflows` | ✓ | Processing and approval definitions |
| `permissions` | ✓ | Module permissions |
| `entitlements` | ✓ | Required entitlements, feature gates, meters |
| `ai` | | AI tasks (capability, profile, prompt, output schema) |
| `evaluation` | | Suites, metrics, release gates |
| `review` | | Default thresholds and bands |
| `ui` | ✓ | Mode, card, labels, extensions |
| `telemetry` | | Metrics, KPIs |
| `translations` | | Locales |
| `x-*` | | Non-normative extensions (ignored) |

## `module`

`key` (`<category>.<name>`, immutable), `version` (SemVer), `publisher` (key), `category`, `name`/`description` (localised, `en` required), `icon`, `tags`, `license`, `support`, `changelog`.

## `inputs`

- `slots[]`: `key`, `label`, `accepts` (source kinds), `mimeTypes` (narrowing), `required`, `multiple`, `requiredExtractions` (extraction kinds), `capture` (`upload`, `record_audio`, `record_video`, `camera`, `scan`, `text`), `offline`.
- `limits`: `maxSources`, `maxTotalBytes`, `maxTotalDurationSeconds`, `maxTotalPages`.
- `readiness`: `minSources`, `requireAnyOfSlots`.
- `autoSubmit` (default `false`): adding a Source never implies submission unless true.

## `knowledge`

- `entityTypes[]`: `key` (`<ns>.<name>`), `baseType` (platform base type), `attributesSchema`, `evidence` (`required`/`recommended`/`none`), `evidenceRequiredAttributes`, `review` (`confidenceThreshold`, `alwaysReview`, `blocksFinalization`).
- `relationTypes[]`: `key` (UPPER_SNAKE), `from`, `to`, `cardinality`.

## `artifacts[]`

`type`, `name`, `schema`, `schemaVersion`, `primary`, `generation` (`automatic` or `on_demand`), `templates[]`, `exports` (`pdf`, `docx`), `approval` (`workflow`, `requirement`: `required`/`optional`/`configurable`), `numbering` (`sequence`, `defaultPattern`, `assignOn`), `createsActionItems` (`fromEntityTypes`, `trigger`: `generated`/`approved`/`final`).

## `ai.tasks[]`

`key`, `capability`, `profile` (model profile key; never a provider/model), `quality`, `languages`, `modalities`, `prompt` (`key@range`), `outputSchema`, `requiresEvidence`, `maxCostCredits`.

## `ui`

`mode` (`generic` forbids `extensions`), `card`, `labels`, `extensions[] {point, component, fallback}` ([module-ui-contract](../module-ui-contract.md)).

## YAML Pitfalls

Manifests are parsed with a YAML 1.2-compatible strict loader. Avoid unquoted keys/values `on`, `off`, `yes`, `no`, `y`, `n` (YAML 1.1 booleans) — this is why the action-item trigger key is named `trigger`. Quote strings containing `,` inside flow mappings and version ranges containing spaces.
