# 11 — Testing and Validation

## Unit tests
YAML/schema/defaults; constant/burst schedule calculations; seeded branch selection; graph validation; sequential order; parallel concurrency/join; dynamic branch/loop limits; timeout/retry/cancel/failure propagation; metrics and JSONL serialization.

## Component tests
Generator sends expected count; schedule within tolerance; max in-flight respected; API backpressure; synthetic executor metadata; valid state transitions.

## Integration tests
Generator → API → orchestrator → executor; one scenario per workflow type; ID/timestamp correlation; failure/timeout; graceful shutdown and result flush.

## Container tests
Reproducible image build; health/readiness; metrics scrape; SIGTERM handling; no secrets baked into image.

## Kubernetes smoke tests
Apply namespace/config/deployment/service; wait for readiness; submit one workflow of each type; inspect logs/events/metrics; verify requests/limits and placement; clean up only explicitly designated test resources.

## Gate before autoscaling experiments
Do not start until generation is validated, workflow semantics are correct, telemetry is complete, generator is not the bottleneck, and results are reproducible.
