# Versioning Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §1.1 "Version everything", §8, §12, §16.1 |

## What Is Versioned and How

| Artifact | Scheme | Immutable unit | Pinned by | Owner doc |
|---|---|---|---|---|
| Module | SemVer (`1.4.2`) | `module_versions` | Installation (policy), Job (at submit) | [module-versioning](../modules/module-versioning.md) |
| Module manifest schema (`manifestVersion`) | `MAJOR.MINOR` | JSON Schema file | Each manifest | [manifest-reference](../modules/sdk/manifest-reference.md) |
| Module schemas (config, job input, knowledge, artifact) | `<module>/<name>@<semver>` | Schema file in package | Data rows via `schema_ref` | [schema-reference](../modules/sdk/schema-reference.md) |
| Prompt | SemVer (`meeting.action-extraction@2.3.1`) | `prompt_versions` | AIExecution, provenance | [prompt-versioning](../ai/prompt-versioning.md) |
| Model profile | Integer `version` + route table | Profile row history (audited) | AIExecution records resolved model | [model-profiles](../ai/model-profiles.md) |
| Artifact content | Ordinal `version_no` | `artifact_versions` | Approvals, exports | [artifact-versioning](../artifacts/artifact-versioning.md) |
| Template | Ordinal | `template_versions` | Exports | [template-engine](../artifacts/template-engine.md) |
| Brand kit | Ordinal | `brand_kits` rows | Exports | [brand-kit](../artifacts/brand-kit.md) |
| Workflow definition | Ordinal | `workflow_versions` | Instances, Jobs | [workflow-architecture](../architecture/workflow-architecture.md) |
| Module configuration | Ordinal | `module_configurations` | Jobs (at submit) | [module-configuration](../modules/module-configuration.md) |
| Plan | Ordinal | `plan_versions` | Subscriptions | [plan-model](../billing/plan-model.md) |
| Evaluation dataset | `key@version` | Dataset version | Evaluation runs | [evaluation-datasets](../ai/evaluation/evaluation-datasets.md) |
| Extraction pipeline | `pipeline_key@version` | `source_extractions` rows | Chunks, evidence | [source-processing](../ingestion/source-processing.md) |
| Public API | URL major (`/v1`) | OpenAPI document version | Clients | [api-versioning](../api/api-versioning.md) |
| Events | Integer per event type | Event JSON Schema | Consumers | [event-architecture](../architecture/event-architecture.md) |
| Data usage policy | Ordinal | `data_usage_policies` rows | Consent records, training examples | [consent](../privacy/consent.md) |
| Platform (for module compatibility) | SemVer of the module runtime contract (`platformApi`) | Release | Manifest `compatibility.platform` | [compatibility-policy](../modules/compatibility-policy.md) |

## Principles

1. Published versions are immutable ([immutable-data-policy](immutable-data-policy.md)).
2. Records that depend on versioned definitions store the exact version ID used (provenance), not "latest".
3. Old versions stay readable for as long as data referencing them exists (historical artifacts must render with their schema).
4. SemVer meaning for module/prompt/schema: MAJOR = breaking contract; MINOR = compatible addition; PATCH = fixes without contract change.
