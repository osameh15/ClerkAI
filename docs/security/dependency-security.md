# Dependency and Supply-Chain Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps + Security |
| Blueprint | BP §14.1 (Supply chain: "SBOM, pinning, signed builds, dependency scanning"), §14.2 |

| Control | Implementation |
|---|---|
| Pinning | `go.sum`, lockfiles (`pnpm-lock.yaml`), container base images pinned by digest |
| Update cadence | Automated update PRs weekly; security updates immediately |
| Scanning | Dependency + license scanning in CI (blocking on Critical/High) |
| SBOM | CycloneDX/SPDX SBOM generated per image, attached to releases |
| Signing | Container images signed (Sigstore/cosign-class); deploy verifies signatures |
| Provenance | Build provenance attestations (SLSA-style) for release artifacts |
| Base images | Minimal/distroless where possible; non-root users |
| Module packages | Hash-verified; future third-party signing ([module-packaging](../modules/module-packaging.md)) |
| Model providers | Pinned model versions in registry ([model-registry](../ai/model-registry.md)) |
