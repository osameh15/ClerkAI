# Meeting Minutes — Configuration

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.1, §4 |

| Key | Type | Default | Description |
|---|---|---|---|
| `language` | enum `auto, fa, en, mixed` | `auto` | Default language hint |
| `speakerIdentification` | enum `none, participants_list, voice_hints` | `participants_list` | Attribution strategy (`voice_hints` future) |
| `approvalRequired` | boolean | `false` | Require approval before Final |
| `approvalWorkflow` | workflow key | `minutes-approval` | Customisable (entitlement) |
| `outputs` | set of artifact types | `[minutes, action_list]` | Automatic artifacts |
| `transcriptVisibility` | enum `members, managers, none` | `members` | Who may see transcripts (combined with permission) |
| `confidenceThreshold` | number 0–1 | `0.8` | Minimum; can be raised only |
| `requireActionOwner` | boolean | `false` | Missing owner becomes blocking |
| `numberingPattern` | string | `MM-{yyyy}-{seq:4}` | Organization numbering |
| `dateCalendar` | enum `jalali, gregorian` | org default | Display calendar |
| `recordingNoticeRequired` | boolean | `true` | Require attestation when recording attached |
| `defaultTemplate` | template key | `minutes-formal` | Export template |
| `glossary` | string[] | `[]` | Org-wide terms for ASR biasing |
| `createActionsOn` | enum `generated, approved, final` | `final` | When Action Items are created |

Scopes: organization, team, project.
