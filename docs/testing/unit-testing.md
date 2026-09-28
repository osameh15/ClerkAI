# Unit Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §19 (Unit: Domain rules, parsers, permissions, pricing calculations) |

## Scope

| Area | Examples |
|---|---|
| State machines | Job transitions table-driven (every allowed + forbidden transition), artifact status, installation, approval instance, action items |
| Authorization | Evaluator truth tables (role × permission × scope × resource state); custom role escalation prevention |
| Entitlements | Resolution/merge rules per type; quota math |
| Pricing | Rate card conversion, overage, proration inputs |
| Knowledge | Canonicalisation (Persian names, Jalali/relative dates), confidence composition, cardinality validation |
| Evidence | Pointer validation, quote matching with Persian normalisation |
| Mappers | Knowledge → artifact mappings; error → API code mapping |
| Parsers/validators | Manifest semantic rules, workflow DAG validation, JSON Patch application |
| Idempotency | Fingerprint canonicalisation |

## Conventions

Table-driven tests; no network/DB; `Clock` and ID generator injected; fuzz tests for parsers (Go native fuzzing) on JSON Patch, manifest parsing, date parsing.
