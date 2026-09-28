# Golden Tests

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + QA |
| Blueprint | BP §19.1, App. D |

## Golden Test Plan per Module (BP §19.1)

| Module | Blueprint focus | Golden scenarios (minimum) |
|---|---|---|
| Meeting | Persian owner/deadline/decision extraction with Evidence | Persian meeting with explicit decisions; mixed fa/en technical meeting; poor audio conference room; overlapping speakers; decision vs proposal vs unresolved distinctions; missing deadline; ambiguous owner (two "Reza"s); agenda contradicting speech; notes-only meeting; Jalali and relative dates; provider failure mid-extraction (mock) |
| Site Report | Image + voice, location/issue/action extraction, repeat inspection | Photo+voice observation groups; voice describing multiple photos; noisy site audio; low-light photos; missing responsible party; critical safety issue; repeat inspection resolved/persisting/worsened; offline-captured batch sync; unsupported file (legacy .doc) |
| Handwriting | Multiple image qualities, ambiguity handling, review flow | Clean scan; skewed phone photo; low-quality/low-light; pencil; mixed printed/handwritten form; Persian digits and Jalali dates; illegible words; crossed-out text; letter generation without recipient |
| Lecture | Audio + slides, concept/definition/example extraction, hallucination control | Persian lecture + English slides; slides-only; audio-only; lecturer contradicts slide; formulas; long (3 h) lecture; source-grounded hallucination probes (forbidden claims) |

## Golden Artefacts

| Artefact | Golden comparison |
|---|---|
| Knowledge extraction | Metrics vs thresholds ([module-evaluations](../ai/evaluation/module-evaluations.md)) |
| Review behaviour | Expected review items (reason codes) present — ambiguity never guessed |
| Evidence | Pointers overlap expected locations |
| Artifact content (deterministic mapping) | Exact JSON snapshot for mock-provider runs |
| PDF/DOCX exports | Visual/structural golden files (no clipping, correct RTL) |

## Execution

PR: smoke subsets (mock + small real subset for AI changes). Nightly/RC: full golden sets. Results stored as evaluation runs with gate results.
