# Lecture Summary — Edge Cases

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |

| Case | Expected behaviour |
|---|---|
| Slides only (no audio) | Summary from slides; alignment step skipped; lower confidence on explanations |
| Audio only | No slide references; topics derived from speech |
| Slides don't match lecture | Low alignment confidence; warning; items cite only matching source |
| Lecturer contradicts slide | `CONTRADICTS`; both shown; review item on definition |
| Heavy math/formulas | Formula entities with high threshold; low confidence → review; LaTeX where extractable |
| Very long lecture (3–4 h) | Segmented processing; topic batching; progress shows segments |
| Student Q&A with multiple speakers | `asked_in_class` questions attributed generically ("student") — no identification of students |
| Poor classroom audio | Lower confidence; summary indicates gaps; no invented content |
| Copyrighted textbook scans | Allowed for personal study; not shared publicly; attestation at upload |
| Enriched mode | Non-source explanations visually labelled and excluded from "grounded" exports unless opted in |
