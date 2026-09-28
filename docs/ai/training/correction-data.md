# Correction Data

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Privacy |
| Blueprint | BP §10.2, §11.3 |

## What Is Captured (always, for product function)

Knowledge corrections (`human_confirmed`, `human_modified`, `rejected`) with before/after values, and artifact edit diffs — stored in the tenant's data under normal retention. This is product data, not training data.

## Candidate Generation (policy-gated)

A nightly job creates **evaluation/training candidates** only for organizations whose `data_usage_policies` allow `evaluation_use` or `training_use` (default false for both):

| Candidate type | Built from |
|---|---|
| Extraction pair | Source chunks + AI output + corrected output |
| Rejection example | Hallucinated/incorrect item + rejection |
| Artifact edit pair | AI draft section + accepted human version |

Candidates start in `candidate` status in `training_examples` ([training-example-lifecycle](training-example-lifecycle.md)); nothing leaves the tenant boundary until approved.

## Withdrawal

If an organization revokes consent, all candidates and examples derived from its data move to `withdrawn` and are purged from datasets in the next dataset version; models already trained are documented in the model card with the data cutoff ([consent-requirements](consent-requirements.md)).
