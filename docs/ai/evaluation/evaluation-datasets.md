# Evaluation Datasets

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §11.3, §11.4, §15 |

## Model

`evaluation_datasets(key, version, module_key, purpose, consent_basis, storage_prefix)` → `evaluation_cases(case_key, input_refs, expected, tags)`. Datasets are immutable per version; changes create a new version.

## Case Structure

| Field | Content |
|---|---|
| `input_refs` | References to input files (platform bucket `evaluation/…`) + job input + module configuration |
| `expected` | Expected Knowledge (entities/relations with attributes), expected evidence locations (tolerances), expected review triggers, forbidden outputs (for hallucination tests) |
| `tags` | `lang:fa`, `lang:en`, `lang:mixed`, `audio:clean`, `audio:poor`, `speakers:multi`, `handwriting`, `scan:low_quality`, `conflict`, `missing_deadline`, `ambiguous_owner`, `unsupported_input`, `provider_failure_sim` |
| `notes` | Annotator notes, adjudication |

## Data Sources (in priority)

1. **Synthetic / staged recordings** created by the team with consent (actors, scripted meetings, staged site visits, handwritten samples by volunteers) — primary for v1.
2. **Public-domain / licensed** material with compatible licences (lectures, documents).
3. **Customer production data** — only via the consent-gated curation pipeline ([training-example-lifecycle](../training/training-example-lifecycle.md)); never by default.

## Annotation

- Two annotators per case for extraction labels; adjudication for disagreements; inter-annotator agreement tracked (Cohen's κ) to know the ceiling.
- Annotation guidelines per module (what counts as a Decision vs Proposal, etc.) are versioned with the dataset.

## Storage and Access

Platform bucket `evaluation/` with restricted access; datasets classified by their most sensitive content; Persian personal names in synthetic data are fictional.
