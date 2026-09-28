# AI Data Usage

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + AI Engineer |
| Blueprint | BP §11.3, §14.1 (Sensitive data disclosure), §15, §15.3 (Vendor Registry) |

## What Is Sent to AI Providers

Only what the task needs: selected chunks/renditions of the current job's Sources, job input context, schema and instructions. Never: other jobs' data (except declared project-scope retrieval), other tenants' data, secrets, user account data beyond names present in content.

## Controls

| Control | Detail |
|---|---|
| Vendor registry | Every provider registered with DPA status, retention, training use, regions, sub-processors |
| Provider settings | No-training / zero-retention options enabled where offered |
| Tenant AI policy | Allowed providers/regions; private AI requirement; payload storage on/off and retention (≤ 30 d) |
| Minimisation | Chunk selection, image downscaling, redaction option for configured patterns (for example national IDs) before sending (Enterprise) |
| Payload storage | Off by default for Enterprise; on (≤ 30 d, encrypted) for debugging otherwise, per policy |
| Transparency | AI Policy screen lists provider categories and settings |
| Default | No customer content used for general model training |
