# Prompt Registry

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11.2 ("prompts / prompt_versions: prompt, input/output schema, lifecycle status") |

## Principle

Prompts are versioned data, not strings scattered in Go code. Go code references prompts by `key@range`; the registry resolves to an immutable PromptVersion.

## Prompt Identity

`key` = `<namespace>.<name>` (for example `meeting.action-extraction`, `site.observation-grouping`, `platform.repair-json`). Namespace equals the module short namespace or `platform`.

## PromptVersion Fields

| Field | Description |
|---|---|
| `version` | SemVer (`2.3.1`), referenced as `meeting.action-extraction@2.3.1` |
| `template` | System + instruction template with typed placeholders (`{{schema}}`, `{{content_blocks}}`, `{{participants}}`) — no string concatenation of untrusted content outside designated delimited slots |
| `input_schema` | JSON Schema of variables |
| `output_schema_ref` | Module/platform JSON Schema of expected output |
| `compatible_profiles` | Profiles this prompt was evaluated with |
| `evaluation_dataset_id` | Dataset (key@version) used for release gating |
| `status` | `draft` → `candidate` → `active` → `deprecated` → `retired` |
| `released_by`, `released_at`, `changelog`, `content_sha256` | Release metadata |

## Sources of Prompts

- Module prompts live in the module package (`prompts/`), registered at module version publication.
- Platform prompts (repair, language detection helpers) live in the platform repository.
- Organizations cannot edit prompts in v1 (template/terminology customisation happens via module configuration variables).

## Change Process

Every change → new PromptVersion → evaluation run against its dataset for each compatible profile → gate result → status `candidate` → canary/flag → `active` ([prompt-versioning](prompt-versioning.md), [regression-gates](evaluation/regression-gates.md)). Prompt changes are auditable (who, when, eval run).
