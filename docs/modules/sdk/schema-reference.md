# Schema Reference

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4 (schemas/), §7.2 (JSONB + schema), §12 |

## Schema Kinds in a Module

| Kind | Path convention | Referenced as | Validates |
|---|---|---|---|
| Configuration | `schemas/configuration.schema.json` | installation config | `module_configurations.config` |
| Job input | `schemas/job-input.schema.json` | job | `jobs.input` |
| Knowledge attributes | `schemas/knowledge/<subtype>.schema.json` or single `knowledge.schema.json` with `$defs` | `<module>/knowledge/<subtype>@<ver>` | `knowledge_entities.attributes` |
| Artifact | `schemas/artifacts/<type>.schema.json` | `<module>/<type>@<ver>` | `artifact_versions.content.body` |
| AI output | `schemas/ai/<task>.schema.json` | prompt output schema | gateway validation |
| UI schema | `*.ui.json` | form rendering | — |

## Rules

- JSON Schema **2020-12**; `$id` = `https://schemas.clerk.ai/modules/<module-key>/<path>@<version>`.
- Artifact schemas compose platform `$defs` from [artifact.schema.json](../../api/schemas/artifact.schema.json) (`richText`, `table`, `personRef`, `dateValue`, `imageRef`) — no raw HTML fields.
- AI output schemas: `additionalProperties: false`, explicit `citations` arrays for evidence-required fields, `confidence` optional, `uncertainty_reason` allowed.
- Knowledge attribute schemas extend base-type attribute schemas (for example `action` requires `title`).
- Any breaking change → MAJOR module version ([module-versioning](../module-versioning.md)).
- `x-ui` annotations allowed for UI hints ([module-ui-contract](../module-ui-contract.md)).

## Example (artifact schema fragment)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://schemas.clerk.ai/modules/business.meeting-minutes/artifacts/minutes@1.0.0",
  "type": "object",
  "required": ["meeting", "decisions", "actions"],
  "properties": {
    "meeting": {
      "type": "object",
      "properties": {
        "date": { "$ref": "https://schemas.clerk.ai/v1/artifact.schema.json#/$defs/dateValue" },
        "participants": { "type": "array", "items": { "$ref": "https://schemas.clerk.ai/v1/artifact.schema.json#/$defs/personRef" } }
      }
    },
    "decisions": {
      "type": "array",
      "items": { "type": "object", "required": ["text"], "properties": { "text": { "$ref": "https://schemas.clerk.ai/v1/artifact.schema.json#/$defs/richText" } } }
    },
    "actions": { "type": "array", "items": { "type": "object" } }
  }
}
```
