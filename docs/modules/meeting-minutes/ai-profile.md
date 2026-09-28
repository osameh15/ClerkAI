# Meeting Minutes — AI Profile

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §3.1, §11, §16 |

## Tasks

| Task | Capability | Profile | Prompt | Output schema | Notes |
|---|---|---|---|---|---|
| `transcribe` (ingestion) | transcription | `persian_transcription` | — | platform transcript | Glossary biasing |
| `diarize` (ingestion) | diarization | `speaker_diarization` | — | platform diarization | |
| `extract_minutes` | structured_extraction | `high_quality_extraction` | `meeting.minutes-extraction@^1.0.0` | `ai/minutes-extraction` | Batched per agenda item / time window; must classify decision vs proposal vs discussion vs unresolved |
| `extract_actions` | structured_extraction | `high_quality_extraction` | `meeting.action-extraction@^1.0.0` | `ai/meeting-actions` | Owner/deadline null when unstated |
| `summarize` | summarization | `standard_generation` | `meeting.executive-summary@^1.0.0` | `ai/executive-summary` | Cites knowledge IDs |

## AI Risk Profile (NIST "Map")

| Aspect | Assessment |
|---|---|
| Intended use | Drafting formal minutes for human review |
| Impact of errors | Misrecorded commitments/decisions → business/legal impact |
| Highest-risk fields | Decision vs proposal classification; action owner; deadline |
| Mitigations | Evidence required; review on low confidence/ambiguity; approval; no silent guessing |
| Languages | Persian, English, mixed; proper nouns |
| Data sensitivity | Confidential (business); may contain PII |
| Human oversight | Review items + optional approval + Draft labelling |

## Persian-Specific Handling

Jalali date expressions («دوازدهم مهر», «هفته آینده», «تا آخر ماه»), honorifics («آقای مهندس احمدی»), ZWNJ normalisation, code-switched technical terms.
