# 07 — Telemetry and Data Model

## Goal
Correlate workflow outcomes with Kubernetes resource/scaling behavior.

## Initial Prometheus metrics
- `workflows_submitted_total{workflow_type,execution_mode}`
- `workflows_completed_total{workflow_type,status}`
- `workflows_in_flight{workflow_type}`
- `workflow_duration_seconds{workflow_type}`
- `workflow_queue_wait_seconds`
- `workflow_rejected_total{reason}`
- `workflow_timeouts_total{workflow_type}`
- `tasks_executed_total{task_type,status,execution_mode}`
- `task_duration_seconds{task_type,execution_mode}`
- `tasks_in_flight{task_type}`
- `llm_requests_total{model,status}` and `llm_request_duration_seconds{model}` only if model label values are bounded
- `llm_input_tokens_total{model}`, `llm_output_tokens_total{model}`

Use histograms for durations. Never use workflow IDs, experiment IDs, prompts, or arbitrary values as Prometheus labels.

## Kubernetes metrics
Deployment desired/current/available replicas; HPA desired/current replicas and metric values; CPU/memory usage and requests/limits; Pod pending/scheduling time; Pod creation-to-Ready time; restarts; node allocatable/utilization; HPA events.

Metrics Server supplies resource metrics commonly used by HPA. Prometheus or another time-series store is needed for historical experiment analysis.

## Event files
`workflow_events.jsonl`:
`timestamp, experiment_id, workflow_id, workflow_type, event, status, scheduled_arrival_at, dispatched_at, accepted_at, started_at, completed_at, duration_ms, queue_wait_ms, step_count, llm_call_count, execution_mode, error_code`

`task_events.jsonl`:
`timestamp, experiment_id, workflow_id, task_id, step_id, task_type, execution_mode, attempt, status, started_at, completed_at, duration_ms, input_tokens, output_tokens, error_code`

`generator_events.jsonl`:
`timestamp, experiment_id, workflow_id, scheduled_arrival_at, dispatched_at, dispatch_lag_ms, http_status, accepted, error_code`

## Timekeeping
Use UTC timestamps for correlation and monotonic clocks for durations. Record separately:
- scheduled arrival;
- actual dispatch;
- API acceptance;
- workflow start/end;
- metric observation;
- HPA desired replica update;
- Pod creation;
- Pod Ready;
- first useful work served by new capacity.

Do not collapse these into an undefined “scaling latency”.

## Artifact layout
```text
results/<experiment_id>/
  manifest.json
  scenario.yaml
  normalized_config.json
  environment.json
  workflow_events.jsonl
  task_events.jsonl
  generator_events.jsonl
  summary.json
  plots/
```
Raw records are immutable; summaries/plots are derived.
