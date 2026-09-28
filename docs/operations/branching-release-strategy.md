# Branching and Release Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §26.1 ("Trunk-based development or short-lived branches; small, reviewable pull requests") |

## Branching

- Trunk (`main`) + short-lived feature branches (≤ 3 days typical).
- Incomplete features merged behind feature flags.
- Conventional commits for changelog generation.

## Releases

| Artefact | Versioning | Cadence |
|---|---|---|
| Platform (api/worker/web) | CalVer or SemVer tag per release (`2026.10.1`) | Weekly to biweekly after beta; on-demand hotfixes |
| Public API | `/v1` + OpenAPI SemVer | With platform releases |
| Modules | SemVer per module | Independent ([publishing-modules](../modules/sdk/publishing-modules.md)) |
| Prompts/profiles | SemVer / profile versions | Via model change management |
| Migrations | Sequential | With platform releases (expand/contract) |

## Hotfix

Branch from release tag → fix → PR to main and cherry-pick → expedited pipeline (all blocking checks still required; eval smoke if AI-related).
