# PDF Export

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Provisional (engine Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain |
| Blueprint | BP §0.1, §12, App. D (Export fidelity) |

## Requirements

| Requirement | Detail |
|---|---|
| Fonts | Embedded subsets; Persian fonts with correct shaping (HarfBuzz-level) |
| Bidi | Correct RTL paragraphs, mixed Persian/English runs, Persian digits option |
| Layout | Header/footer, page numbering "x of y", repeated table headers, no clipping of long tables/words |
| Images | Site photos with captions and evidence references; resolution-capped for size |
| Metadata | Title, author (organization), document number, creation date; no internal IDs in visible content |
| Accessibility | Tagged PDF where engine supports (target PDF/UA-friendly) |
| Archival | PDF/A-2b option for final documents (proposed) |
| Signatures | Visual signature blocks; cryptographic PDF signing is Future |
| Determinism | Stable output for golden tests (fixed creation date in test mode) |

## Engine Decision

[OD-008](../governance/open-decisions.md#od-008). Candidate evaluation criteria: Persian shaping quality, determinism, sandboxability, performance (target p95 < 10 s for 20-page report), licence.

## TBD

Engine selection and PDF/A default — decide at G10 with golden-sample bake-off.
