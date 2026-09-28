# Provider Contract

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11.1, §11.2, App. F ("at least two provider/model adapters, or one provider plus a mock/fallback adapter") |

## Port

```go
type Provider interface {
    Key() string
    Capabilities() []Capability
    Invoke(ctx context.Context, call ProviderCall) (ProviderResponse, error)
}

type ProviderCall struct {
    Model          string            // provider model id from registry
    Capability     Capability
    Messages       []Message         // system / user segments with content parts
    Media          []MediaRef        // presigned URLs or uploaded file refs for audio/image/video
    ResponseSchema json.RawMessage   // if provider supports native structured output
    Params         map[string]any    // temperature, max tokens, language hints, speaker count
    Timeout        time.Duration
    TenantHash     string            // opaque per-org identifier for provider-side abuse tracking
    NoTrain        bool              // provider-side no-training/zero-retention flags where available
}

type ProviderResponse struct {
    Output       json.RawMessage     // or text for transcripts (normalized to JSON)
    FinishReason string
    Usage        Usage               // input/output tokens, audio seconds, images, pages
    ProviderRequestID string
    Raw          []byte              // only kept if payload retention enabled
}
```

## Error Classification (required of every adapter)

| Class | Examples | Gateway behaviour |
|---|---|---|
| `transient` | 429, 5xx, timeout, connection reset | Retry same route with backoff |
| `capacity` | quota exhausted at provider | Open circuit, fallback |
| `invalid_request` | context too long, unsupported media | Try route with larger context / different modality handling, else fail |
| `content_rejected` | safety refusal | Fail step with `AI_CONTENT_REJECTED` (no silent fallback to less safe model unless policy allows) |
| `auth` | credential invalid | Alert, open circuit, fallback |
| `unknown` | anything else | Treat as transient once, then fail |

## Adapter Requirements

- Normalise usage units; map provider pricing via the model registry (cost computed by the gateway, not the adapter).
- Never log payloads; attach `provider_request_id` to AIExecution.
- Support cancellation via context.
- Media transfer: prefer short-lived presigned URLs to renditions when the provider fetches remotely, else stream bytes; never make buckets public.
- Contract tests with recorded fixtures per adapter; a **mock provider** returning schema-valid deterministic outputs is mandatory for local/CI ([ai-testing](../testing/ai-testing.md)).

## Candidate Providers

OpenAI, Google, Anthropic, local/open-weight models, specialised ASR/OCR vendors — selection per [OD-015](../governance/open-decisions.md#od-015), [OD-016](../governance/open-decisions.md#od-016), with data-processing terms recorded in the vendor registry.
