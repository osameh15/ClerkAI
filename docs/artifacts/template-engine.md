# Template Engine

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (format Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Templates domain |
| Blueprint | BP §12 ("Template: Layout is independent from content"), App. A (Templates), App. C (Custom Brand/Template) |

## Separation

```text
Content (ArtifactVersion JSON)  +  Template (layout, versioned)  +  Brand Kit (identity, versioned)  →  Renderer  →  Web / PDF / DOCX
```

Module code never implements PDF layout; modules ship default templates as data in their package.

## Template Model

- `templates`: identity (`key`, `artifact_type`, `module_key`, owner org or null for module-provided).
- `template_versions`: immutable published versions with `engine`, `definition_ref`, compatible artifact schema range, supported formats.
- Organizations (entitlement `template.custom`) can clone a module template and customise within constraints: section order/visibility, headings text, cover page, table styles, signature blocks, numbering placement. Arbitrary code/scripts are not allowed.

## Definition Format

[OD-038](../governance/open-decisions.md#od-038). Requirements for any choice:
- Declarative slots bound to envelope paths (`metadata.date`, `body.decisions[]`).
- Loops and conditionals over structured content only; no network/file access.
- One template definition renders to both PDF and DOCX, or a template declares separate format-specific layouts sharing slots.
- RTL/LTR and bilingual labels.
- Deterministic output for golden tests.

## Selection

Export request may specify `template_key`; otherwise the organization default for the artifact type, else the module default. The chosen `template_version_id` is recorded on the export.
