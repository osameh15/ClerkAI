# Site Report — Configuration

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.2 |

| Key | Type | Default | Description |
|---|---|---|---|
| `categories` | string[] | structural, electrical, mechanical, safety, quality, housekeeping, other | Category vocabulary (localised labels) |
| `severities` | string[] | low, medium, high, critical | Severity scale |
| `defaultDueDays` | map severity→days | {low:14, medium:7, high:3, critical:1} | Deadline default when unstated (labelled `severity_default`) |
| `locationVocabulary` | per project list | [] | Buildings/floors/zones for normalisation |
| `requireProject` | boolean | true | Visits must belong to a project |
| `approvalRequired` | boolean | true | Formal approval |
| `approvalWorkflow` | key | site-report-approval | Customisable |
| `groupingWindowSeconds` | integer | 120 | Capture-time window for auto grouping |
| `includeGps` | boolean | false | Include GPS in report/exports |
| `confidenceThreshold` | number | 0.75 | Min threshold (raise only) |
| `alwaysReviewSeverities` | string[] | [critical] | Always create review items |
| `numberingPattern` | string | SR-{project}-{yyyy}-{seq:4} | |
| `photoPolicy` | enum `all, observations_only` | observations_only | Which photos appear in report |
| `offlineRetentionDays` | integer | 14 | Max days captures may remain local unsynced before warnings |

Scopes: organization, project.
