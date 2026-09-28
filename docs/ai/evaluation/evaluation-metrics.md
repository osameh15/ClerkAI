# Evaluation Metrics

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §11.4, App. D |

## Metric Definitions

| Key | Definition |
|---|---|
| `<type>.precision` / `.recall` / `.f1` | Item-level matching against expected items. Match rule per type: semantic match (normalised text similarity ≥ threshold **and** LLM-judge or annotator-defined key attributes) — matcher version recorded |
| `owner.accuracy` | Correct owner (member or text) among matched actions |
| `deadline.accuracy` | Correct canonical date among matched actions with deadlines (tolerance 0 days) |
| `decision.f1` | F1 of decisions; proposals mislabelled as decisions count as false positives |
| `speaker.attribution_accuracy` | Correct speaker→participant mapping on matched statements; ambiguous cases must produce review items (counted as correct abstention) |
| `evidence.coverage` | Key facts with ≥ 1 valid evidence / key facts |
| `evidence.alignment` | Evidence pointers overlapping expected locations (time IoU ≥ 0.5, bbox IoU ≥ 0.5, page exact) / evidence pointers |
| `hallucination.unsupported_claim_rate` | Output items with no support in sources (judge + citation check) / output items |
| `hallucination.critical_unsupported_action_rate` | Unsupported actions with owner or deadline / actions |
| `hallucination.silent_invention_rate` | Unreadable spans emitted as confident text / unreadable spans |
| `hallucination.out_of_source_claim_rate` | Lecture: study-material claims not grounded in sources / claims |
| `transcription.wer_fa`, `.cer_fa`, `.wer_en` | Standard WER/CER after normalisation (Persian normalisation rules) |
| `transcription.proper_noun_accuracy` | Correct proper nouns / expected proper nouns |
| `diarization.der` | Diarization error rate |
| `ocr.cer_fa`, `ocr.cer_en` | Character error rate |
| `uncertain.recall` | Truly illegible/uncertain spans flagged / truly uncertain spans |
| `numbers_dates.accuracy` | Exact numbers/dates correct |
| `observation.media_linkage_accuracy` | Photo/voice correctly attached to observation |
| `category.accuracy`, `severity.accuracy`, `location.accuracy` | Site Report attribute accuracy |
| `reinspection.match_accuracy` | Correct match of new observation to previous open issue |
| `concept.recall`, `definition.accuracy` | Lecture |
| `flashcard.answerable_from_source_rate` | Flashcards whose answers are supported by sources |
| `schema.validity_after_retry` | Schema-valid final outputs / calls |
| `artifact.edit_distance` | Normalised structural edit distance between AI draft and accepted version (production/human eval) |
| `calibration.ece` | Expected calibration error of confidence |
| `cost.per_case_micros`, `latency.p95_ms` | Economics |

## Baseline Targets from Blueprint App. D (initial, to recalibrate)

| Area | Metric | Target |
|---|---|---|
| Core | Schema validity after retry/fallback | ≥ 99.5% |
| Meeting | Critical unsupported actions | < 1% |
| Meeting | Evidence (key facts openable) | ≥ 90% |
| Meeting | Action/Owner/Deadline | Separate P/R goals (see module plan) |
| Meeting | Ambiguous speaker | Review Item, not guess |
| Handwriting | Silent invention | 0 (no unreadable field enters an official document as guessed text) |
| Lecture | Concept grounding | Core concepts linked to evidence |
| Export | Golden PDF/DOCX | No clipping or broken layout |

Module-specific thresholds: [module-evaluations](module-evaluations.md).
