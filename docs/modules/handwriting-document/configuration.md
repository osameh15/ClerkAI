# Handwriting → Document — Configuration

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |

| Key | Type | Default | Description |
|---|---|---|---|
| `defaultOutput` | enum | `clean_document` | Automatic artifact |
| `preserveLineBreaks` | boolean | false | Keep original line breaks vs reflow paragraphs |
| `normalizeSpelling` | boolean | false | Allow orthographic normalisation (never semantic changes) |
| `unreadablePolicy` | enum `block_finalization`, `allow_marked_illegible` | `block_finalization` | Both forbid silent guesses; second allows `[illegible]` markers in Final |
| `confidenceThreshold` | number | 0.85 | Raise only |
| `numbersDatesThreshold` | number | 0.90 | Raise only |
| `letterApprovalRequired` | boolean | false | |
| `letterNumberingPattern` | string | `L-{yyyy}-{seq:5}` | |
| `defaultLetterTemplate` | key | `formal-letter-default` | |

Scopes: organization, team.
