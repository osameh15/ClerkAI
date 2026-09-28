# Privacy Architecture

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy (Founder + Tech Lead until dedicated role) |
| Blueprint | BP §1.1 (Privacy by design), §15, R10 (GDPR principles where applicable) |

## Stable Principles (BP §15)

Purpose limitation · data minimisation · retention control · transparency · access control · export/deletion workflows. Architecture must adapt to jurisdiction-specific requirements; where users are covered by GDPR, applicable rights and principles are supported.

## Default Principle

> **Customer content must not automatically be used for general model training.** Enterprise organizations can restrict data usage further.

## Building Blocks

| Block | Document |
|---|---|
| Classification | [data-classification](data-classification.md) |
| Inventory (what we hold, where, why) | [data-inventory](data-inventory.md) |
| Retention (configurable per category) | [data-retention](data-retention.md) |
| Deletion (async, audited) | [deletion](deletion.md) |
| Export (usable format) | [export](export.md) |
| Consent (recording notice, AI data use) | [consent](consent.md) |
| AI data usage (providers, payloads) | [ai-data-usage](ai-data-usage.md) |
| Training data policy | [training-data-policy](training-data-policy.md) |
| Regional considerations | [regional-data-considerations](regional-data-considerations.md) |
| Enterprise controls | [enterprise-data-controls](enterprise-data-controls.md) |

## Roles

For customer content Clerk acts as **processor** on behalf of the organization (controller); for account/billing data Clerk acts as controller. Data processing terms and sub-processor list (vendor registry) are maintained (BP §15.3).
