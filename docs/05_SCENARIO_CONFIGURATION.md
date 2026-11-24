# 05 — Scenario Configuration

YAML is declarative input to the runner/generator. It does not generate traffic by itself.

## Initial schema example
```yaml
api_version: workload.platform/v1alpha1
metadata:
  name: sequential-low-load
  description: Sequential synthetic workflow at constant open-loop rate
  seed: 42
workload:
  mode: open_loop
  workflow_type: sequential
  total_workflows: 60
  duration_seconds: null
  arrival:
    type: constant_rate
    rate_per_second: 1.0
  max_in_flight: 10
  overload_policy: wait
  warmup_seconds: 10
  drain_timeout_seconds: 60
  cooldown_seconds: 5
workflow:
  execution_mode: synthetic
  timeout_seconds: 120
  max_retries: 0
  task_profile:
    synthetic_delay_ms: 400
    input_tokens_estimate: 500
    output_tokens_estimate: 150
  sequential:
    steps:
      - id: analyze
        task_type: analysis
      - id: evaluate
        task_type: evaluation
      - id: compose
        task_type: generation
observability:
  prometheus: true
  export_workflow_events: true
  export_task_events: true
output:
  base_directory: results
  format: jsonl
```

## Validation rules
- Supported schema version.
- Require total count and/or bounded duration with explicit semantics.
- Positive rate for constant-rate mode.
- `max_in_flight >= 1`.
- Workflow type must match graph definition.
- Unique step/branch IDs.
- Non-negative delay/token estimates; positive timeout.
- Finite retry and loop bounds.
- Supported execution/overload modes.
- Reject unknown keys in strict mode.
- Use `yaml.safe_load`, never unsafe YAML constructors.

Store original YAML and normalized configuration with every run. Version schema changes; do not silently reinterpret old files.

See `examples/scenarios/` for illustrative scenarios. Their values are not final benchmark parameters.
