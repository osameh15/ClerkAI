# Site Report — AI Profile

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §3.2, §11, §16 |

## Tasks

| Task | Capability | Profile | Prompt | Notes |
|---|---|---|---|---|
| `transcribe_voice` | transcription | `persian_transcription` | — | Noisy site audio |
| `analyse_media` | vision_analysis | `high_quality_vision` | `site.media-analysis@^1.0.0` | Per image/frame: visible defects, conditions, safety elements, visible text; bbox regions |
| `group_observations` | structured_extraction | `high_quality_extraction` | `site.observation-grouping@^1.0.0` | Fuses voice + photo + text into observations/issues with categories, severities, locations, actions, parties, deadlines |
| `match_reinspection` | structured_extraction | `high_quality_extraction` | `site.reinspection-matching@^1.0.0` | Proposes matches + outcomes with evidence |

## AI Risk Profile

| Aspect | Assessment |
|---|---|
| Intended use | Draft inspection report for engineer review |
| Impact of errors | Safety issues missed or under-rated; wrong responsible party |
| Highest-risk fields | Severity (esp. critical), resolved status, responsible party |
| Mitigations | Critical severity always reviewed; "resolved" always human-confirmed; approval default on; evidence required |
| Bias risk | Photo-only severity estimation is unreliable → severity suggestions labelled; voice statements weigh more |
| Data | Confidential; may include faces/licence plates → blurred in exports optional (config) |
