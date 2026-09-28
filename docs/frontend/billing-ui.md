# Billing and Usage UI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (provider checkout Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product |
| Blueprint | BP §5, §20.2 (UsageMeter), App. A (Billing & Usage) |

## Screens

| Section | Content |
|---|---|
| Plan | Current plan, period, seats used/purchased, add-ons, status (trialing/past_due warnings) |
| Usage | UsageMeters for AI Credits, Processing Minutes, Pages, Storage (used vs included vs limit), projected overage, by-module breakdown |
| Budget | Organization AI budget and alert thresholds (billing.manage) |
| Invoices | List/download (provider-hosted links) |
| Upgrade | Plan comparison from PlanVersions (entitlement matrix), checkout via provider ([OD-010](../governance/open-decisions.md#od-010)) |

## Rules

Never show tokens; show customer units only. Quota warnings appear contextually (job submit, upload). Members without `billing.view` see only their module-level quota messages.
