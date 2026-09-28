# Review Process

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Future (M1) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + Security |
| Blueprint | BP §23.1 (M1: partner declarative modules with manual review) |

## Review Checklist (M1)

| Area | Checks |
|---|---|
| Contract | Manifest validation L1–L8; no native/remote runtime |
| Permissions | Minimal; namespace-correct; justified |
| Data | Declared AI data policy; no instructions to exfiltrate; prompts reviewed for injection resilience |
| Quality | Evaluation suites present and passing on publisher-provided + Clerk-held test sets |
| UX | Generic UI mode only (no custom components) |
| Legal | Licence, content rights, pricing terms |
| Security | Templates free of active content; package integrity/signature |

Outcomes: approve, request changes, reject; every published version re-reviewed (diff-based for minor/patch).
