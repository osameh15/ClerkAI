# Audio Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (providers Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain + AI Engineer |
| Blueprint | BP §3.1, §9 ("validation → normalize → transcription → diarization → timestamps"), §11.4 (WER/CER), §27 (Persian AI quality) |

## Pipeline

| Step | Implementation | Output |
|---|---|---|
| Validation | ffprobe (sandboxed): codec, duration, channels, sample rate; reject corrupt/zero-length | `metadata` |
| Normalise | ffmpeg → mono 16 kHz FLAC/Opus rendition, loudness normalisation; long files segmented (for example 10-min windows with 5 s overlap) | `normalized_audio` rendition |
| Transcription | AI Gateway `transcription` capability, profile by language hint / detected language (`persian_transcription`, `english_transcription`, mixed) | `transcript` with word/segment timestamps + confidences |
| Diarization | `diarization` capability (`speaker_diarization`), aligned with transcript segments | `diarization` (speaker labels S1..Sn per segment) |
| Language detection | Per segment (supports code-switching fa/en) | `language` |
| Timestamps | Segment and word-level timestamps preserved in chunks | chunks (`time_range`) |

## Persian and Mixed Content

- Language routing per segment; mixed fa/en segments routed to profiles evaluated for code-switching.
- Proper nouns and domain terms: optional per-job vocabulary hints (participant names, project terms) passed to providers that support biasing.
- Evaluation: WER/CER with Persian focus and proper-noun accuracy ([evaluation-metrics](../ai/evaluation/evaluation-metrics.md)).

## Speaker Attribution

Diarization yields anonymous labels. Mapping labels to participants happens in the **module** (Knowledge fusion with participant list / agenda) — ambiguous mappings become Review Items, never guesses (BP App. D).

## Live Recording

v1: browser records locally and uploads in chunks (multipart); processing starts after stop ([OD-036](../governance/open-decisions.md#od-036)).

## Cost and Limits

Meter `audio.transcription` (seconds) per AIExecution; `upload.max_duration_seconds` enforced; long audio processed in parallel segments with idempotent segment activities.

## Failure Modes

Silence/no speech → empty transcript with `quality.no_speech=true` (not an error); poor audio → low confidences → review items; provider outage → fallback profile route ([fallback-strategy](../ai/fallback-strategy.md)).
