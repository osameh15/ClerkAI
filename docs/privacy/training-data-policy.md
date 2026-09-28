# Training Data Policy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + AI Engineer |
| Blueprint | BP §11.3, §11.5 |

## Policy Statements

1. Customer data is **not** used for general model training by default.
2. Every organization has a clear, visible policy setting (evaluation use, training use), default off.
3. A production correction becomes a Training Example only after review, appropriate consent/legal basis, and anonymisation where possible.
4. Enterprise organizations can contractually prohibit all AI data use beyond processing.
5. Training examples are versioned; withdrawal removes them from future dataset versions.
6. Fine-tuning providers must contractually not reuse data.
7. Model cards document data sources, consent basis, cutoff and known limitations.

Process: [training-example-lifecycle](../ai/training/training-example-lifecycle.md), [consent-requirements](../ai/training/consent-requirements.md), [anonymization](../ai/training/anonymization.md).
