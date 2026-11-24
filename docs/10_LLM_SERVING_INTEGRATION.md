# 10 — Optional LLM Serving Integration

## Role
LLM serving is a task-execution dependency. Keep its adapter separate from workflow graph logic. No engine/model/GPU is confirmed yet.

## Candidate backends
vLLM, SGLang, or external hosted inference API. Select only after checking worker GPU availability, driver/device-plugin state, model size, and available memory.

## Adapter responsibilities
- Build request from task context.
- Apply connection/read/write timeouts.
- Handle rate limits and retryable failures with bounded retries.
- Record model/engine identity, latency, TTFT if available, token counts, and status.
- Redact prompts and sensitive outputs from logs by default.
- Normalize backend response into common `TaskResult`.

## Synthetic versus real
Synthetic: deterministic delay and optional controlled CPU work; token values are estimates; does not reproduce GPU inference.
Real: actual inference request; capture real tokens/latency; record model, quantization, engine version, hardware, and serving settings.
Keep modes separate in manifests and analysis.

## Open questions
GPU availability/type; chosen model and engine; in-cluster versus external serving; whether inference capacity is held fixed while scaling workflow Pods.
