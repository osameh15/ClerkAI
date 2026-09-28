# Module Sandboxing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Future (v1: not applicable beyond official modules) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Module Platform |
| Blueprint | BP §1.2, §4.2 (Remote: sandboxed permissions, no direct DB access), §23 |

## v1 Position

Only official modules; declarative + reviewed native steps; **no loading of third-party code (for example Go `.so` plugins) into the primary process.** Module security: [module-security](../modules/module-security.md).

## Future Remote Module Security Model

| Control | Design |
|---|---|
| Execution location | Publisher-hosted endpoint (or Clerk-hosted isolated runtime in a separate account/cluster) |
| Remote Module Gateway | Only path between Clerk and remote code |
| Request signing | Clerk signs requests (asymmetric key per environment); module signs responses with registered key |
| Capability scopes | Declared `dataScopes` (job input, normalized content, knowledge read/write, artifact write) approved at install by admin |
| Data minimisation | Gateway sends only declared scope data for the job; no tenant-wide access |
| No DB access | Remote modules never receive credentials; writes go through gateway validation (schemas) |
| Rate limits and timeouts | Per module and per tenant |
| Audit | Every request/response logged (metadata) and audited as `remote_module` actor |
| Kill switch | Suspend module globally or per tenant |
| Review | Publisher verification + security review before listing (M2) |
| UI | Third-party UI only in sandboxed iframes with postMessage contract (future) |
