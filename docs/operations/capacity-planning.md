# Capacity Planning

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §6 (Kubernetes only when justified), §19 (Load tests) |

## Drivers

Media minutes/day (ASR cost/time), pages/day (OCR), concurrent jobs, upload bandwidth, storage growth (raw media dominates), DB growth (chunks, usage/audit events), AI provider rate limits.

## Initial Assumptions (to validate in beta)

| Metric | Beta assumption |
|---|---|
| Active orgs | 20–50 pilot orgs |
| Jobs/day | 200–1,000 |
| Media | ≤ 300 h/month |
| Storage | ≤ 2 TB first 6 months (raw media retention 90 d) |

## Scaling Levers

Worker pool replicas per queue; provider rate-limit increases and multi-provider routing; PostgreSQL vertical scaling + read replicas; partitioning of high-volume tables when thresholds hit ([partitioning-strategy](../data/partitioning-strategy.md)); retention of raw media.

## Review Cadence

Monthly capacity review from Private Beta with load-test results ([load-testing](../testing/load-testing.md)).
