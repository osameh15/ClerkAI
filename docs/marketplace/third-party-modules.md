# Third-Party Modules

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Future |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §1.2 ("In v1, it is not a platform that executes arbitrary third-party code inside the main process"), §23 |

## Allowed Forms by Stage

| Stage | Form | Execution |
|---|---|---|
| M1 | Declarative partner modules (manifest, schemas, prompts, templates, workflows from platform step types) | Platform interpreter; no code |
| M2 | Remote modules | Publisher endpoints via Remote Module Gateway ([remote-modules](remote-modules.md)) |
| M3 | Public SDK | Tooling for declarative + remote modules; automated validation |

**Never:** loading third-party binaries/plugins into Clerk processes.

## Constraints for Third-Party Declarative Modules

Generic UI only; no `native` steps; AI tasks limited to catalogued profiles; entitlement keys issued by Clerk; data access limited to their jobs.
