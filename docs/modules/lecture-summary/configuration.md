# Lecture Summary — Configuration

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |

| Key | Type | Default | Description |
|---|---|---|---|
| `groundingMode` | enum `source_grounded`, `enriched` | `source_grounded` | Enriched adds clearly labelled non-source explanations |
| `summaryLength` | enum `brief`, `standard`, `detailed` | `standard` | |
| `flashcardCount` | integer 5–100 | 20 | |
| `questionTypes` | set | `[recall, conceptual]` | |
| `language` | enum | `auto` | Output language (default = dominant source language) |
| `bilingualTerms` | boolean | true | Show English term alongside Persian concept names |
| `includeTimestamps` | boolean | true | Link sections to lecture timestamps |
| `approvalRequired` | boolean | false | |

Scopes: organization, team.
