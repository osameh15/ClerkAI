# Plan Entitlements (v0)

| Field | Value |
|---|---|
| Status | Accepted (structure) / Provisional (values) |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product / Founder |
| Blueprint | App. C "Proposed Plan Entitlement Matrix v0", §5.1 |

**Validation Pricing / Provisional.** Values are hypotheses to revise after 4–6 weeks of Cost-per-Job data ([OD-021](../../governance/open-decisions.md#od-021)). They live in PlanVersions, never in code.

## Blueprint Matrix (App. C)

| Feature | Personal | Team | Business | Enterprise | Entitlement key(s) |
|---|---|---|---|---|---|
| Base General Modules | ✓ | ✓ | ✓ | ✓ | `module.general.*` |
| Four Launch Modules | Limited | ✓ | ✓ | ✓ | `module.<key>` = limited/full |
| Custom Brand / Template | — | ✓ | ✓ | ✓ | `template.custom` |
| Custom Roles | — | Limited | ✓ | ✓ | `roles.custom` |
| Approval Workflow | — | Limited | ✓ | ✓ | `workflow.approval` |
| Audit Log | — | Limited | ✓ | ✓ | `audit.log` |
| Audit Export | — | — | Limited | ✓ | `audit.export` |
| API / Webhooks | — | Limited | ✓ | ✓ | `api.access`, `api.webhooks` |
| SSO / SAML | — | — | — | ✓ | `sso.saml` |
| Custom Retention | — | — | Limited | ✓ | `retention.custom` |
| Private AI / Data Policy | — | — | Limited | ✓ | `ai.private_policy` |
| SLA / Dedicated Support | — | — | — | ✓ | `support.sla` |
| Construction Pack | Add-on | Add-on | Add-on | Contract / Add-on | `pack.construction` |

Seat scope (BP §5.1): Personal 1 user; Team up to 5; Business up to 20; Enterprise custom → `seats.max`.

## Technical Consequences by Plan Dimension

| Dimension | Technical consequence |
|---|---|
| Seats | Invitation acceptance checks `seats.max` (`ORG_SEAT_LIMIT_REACHED`); seat count from active memberships (guests excluded — provisional). |
| Installed modules | Install checks `modules.installed.max` and `module.<key>`. |
| Storage | Upload session creation checks projected `storage.bytes`; retention sweeps free space. |
| AI usage | Gateway reserves AI credits before execution; hard limit blocks unless `overage.allowed`. |
| Audio processing | `processing.minutes.monthly` checked at submit using Source durations (estimate) and reconciled after. |
| Custom templates | Template create/edit endpoints gated by `template.custom`. |
| Approval workflows | `submit-for-approval` gated by `workflow.approval`; `limited` = single-stage flows only (proposed). |
| Audit features | `/v1/audit` gated by `audit.log` (`limited` = 30-day lookback, proposed); export gated by `audit.export`. |
| API | API key creation gated by `api.access`; `limited` = 1 key, lower rate limits (proposed). |
| SSO | IdP configuration gated by `sso.saml` (architecture-ready; implementation on demand). |
| Enterprise controls | Retention overrides, provider restrictions, payload retention disable gated by `retention.custom`, `ai.private_policy`. |

"Limited" values marked *proposed* require product confirmation before G12.
