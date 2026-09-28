# Module Sandbox (Marketplace View)

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Future |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security |
| Blueprint | BP §4.2, §23 |

Security model: [security/module-sandboxing](../security/module-sandboxing.md).

## Additional Marketplace Aspects

- **UI sandbox (future):** third-party UI only in sandboxed iframes on a separate origin, communicating via a typed postMessage bridge exposing a minimal SDK; CSP isolates from the app origin.
- **Test sandbox for publishers:** a developer organization with mock AI provider and synthetic data for module development (M3).
- **Resource quotas per module:** calls, payload sizes and execution time budgets per tenant.
