# Compatibility Policy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4.1 (Upgrade Required), §8 |

## Platform Module Runtime Contract Version (`platformApi`)

The platform exposes a SemVer'd module runtime contract (manifest format, step types, capabilities, UI extension points). Current: **1.0.0**.

| Platform change | Contract bump | Module impact |
|---|---|---|
| New step type / capability / extension point / optional manifest field | MINOR | None |
| Bug fix in step behaviour | PATCH | None |
| Removal/semantic change of step type, capability or manifest field | MAJOR | Modules must declare compatibility; deprecation window ≥ 6 months for third parties (future), aligned with official module releases |

Manifests declare `compatibility.platform` (for example `>=1.0.0 <2.0.0`). The platform refuses to install or execute versions whose range excludes the current contract version.

## Support Window

- The platform supports the current and previous MAJOR contract during transitions.
- Versions scheduled to lose support are marked Upgrade Required with a date.

## Data Compatibility

Historical data remains readable indefinitely (subject to retention) regardless of module version support: schemas, templates and renderers for referenced versions are retained.
