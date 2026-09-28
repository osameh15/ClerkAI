# Module Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (v1) / Future (third-party) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Module Platform |
| Blueprint | BP §1.2, §4.2, §14.1 (Supply chain), §23 |

## v1 (official modules)

| Control | Implementation |
|---|---|
| No arbitrary code in main process | Declarative runtime; native steps are reviewed platform code; no `.so` plugins ([ADR-008](../governance/adr/ADR-008-declarative-first-modules.md)) |
| Capability scopes | Steps receive facades scoped to the job's tenant, job, and declared slots/entity types |
| Data scopes | A module reads only its job's sources/knowledge (plus declared project-scope links); cannot query other modules' data |
| No direct DB/network/provider access | Enforced by construction (no such facades) |
| Package integrity | Hashes verified at load |
| Prompt safety | Prompts reviewed; injection-resistant templates ([prompt-injection](../security/prompt-injection.md)) |
| Templates | No scripts; sandboxed renderer |
| Review | Security review checklist in publication for new step types/permissions |
| Audit | Install/upgrade/config actions audited |

## Future (third-party modules)

Remote Module Gateway: signed requests, per-module credentials, rate limits, timeouts, audit of every call, data-scope minimisation, no direct DB access, no unrestricted tenant access; publisher verification and review; suspension kill-switch ([module-sandboxing](../security/module-sandboxing.md), [remote-modules](../marketplace/remote-modules.md)).
