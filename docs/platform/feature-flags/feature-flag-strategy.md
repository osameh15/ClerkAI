# Feature Flag Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (implementation Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §11.4, §16.1 ("Large changes use feature flags/canary rollout"), §29.2 ("Feature flag/rollback exists for high-risk changes") |

## Purpose

Runtime/deployment controls for gradual rollout, canaries, kill switches and experiments. **Not** a substitute for entitlements ([feature-gates](../entitlements/feature-gates.md)).

## Targeting

| Level | Example |
|---|---|
| Global | `ai.new-router` on/off |
| Organization | enable `editor.v2` for pilot orgs |
| User | internal testers |
| Percentage rollout | 10% of organizations (sticky by `organization_id` hash) |
| Environment | staging-only flags |

## Flag Types

| Type | Lifetime | Example |
|---|---|---|
| Release | Remove ≤ 2 releases after 100% | `editor.v2` |
| Ops / kill switch | Long-lived | `ai.provider.openai.enabled`, `exports.docx.enabled` |
| Experiment | Time-boxed | `review.queue.layout-b` |
| Permission-like | **Forbidden** — use entitlements/permissions | — |

## Rules

1. Flags are evaluated server-side; the client receives only flags relevant to UI rendering for the current context.
2. Every AI model/prompt/router change goes out behind a flag or canary (BP §16.1).
3. Flag default is the safe/old behaviour; code handles both paths until removal.
4. Changes to production flags are audited (`platform` scope) and require a reason.
5. Stale-flag report each sprint; release flags older than 60 days are tech debt ([technical-debt-policy](../../execution/technical-debt-policy.md)).
6. Implementation behind an OpenFeature-compatible interface ([OD-017](../../governance/open-decisions.md#od-017)); `platform/featureflag` port.

## Examples

`editor.v2`, `ai.new-router`, `module.third-party-beta`, `ingestion.video.scene-detection`, `pwa.background-sync`.
