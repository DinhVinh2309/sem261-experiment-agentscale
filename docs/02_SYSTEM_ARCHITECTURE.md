# 02 — System Architecture

```text
Researcher / CLI
      |
Experiment Runner <--- Scenario YAML
      |
Scenario Loader + Validator
      |
Workload Generator (arrival schedule, rate, concurrency)
      | HTTP submissions
Workflow API (FastAPI)
      |
Workflow Orchestrator
   |          |           |
Sequential  Parallel   Dynamic
   \          |          /
        Task Executor
        /           \
 Synthetic Adapter  LLM Adapter (optional)
                          |
                   vLLM/SGLang/provider TBD

Application logs/metrics -> Prometheus / result files
Kubernetes metrics/events -> Metrics Server / Prometheus
Workflow/task events -> JSONL/CSV artifacts
```

## Component responsibilities
- **Experiment Runner:** CLI; creates experiment ID; loads scenario; captures git/environment metadata; starts generator; waits for drain; exports manifest/summary.
- **Scenario Loader:** safe YAML parse; Pydantic validation; defaults; cross-field checks; normalized immutable config.
- **Workload Generator:** open-loop arrival scheduling independent of workflow completion; creates IDs/payloads; enforces max in-flight and backpressure; records scheduled vs actual dispatch.
- **Workflow API:** validates submissions; applies idempotency/capacity policy; returns `202 Accepted` for queued asynchronous work; exposes status/result.
- **Orchestrator:** workflow semantics, dependencies, timeouts, retries, cancellation, task records.
- **Task Executor:** replaceable synthetic/LLM/tool adapters.
- **Observability:** bounded-cardinality Prometheus metrics, structured logs, per-workflow/task JSONL.
- **Kubernetes:** runs services and provides resource/scaling behavior.

## Recommended initial topology
- `workflow-api`: Deployment + ClusterIP Service.
- `workload-generator`: researcher workstation first, or one Job per run after validating cluster resource impact.
- `llm-serving`: separate endpoint/deployment if used.
- Reuse existing Prometheus/Grafana if available; do not install duplicates blindly.
- Store results on runner host initially; consider PVC/object storage only if required.

## Scaling boundary
Explicitly state which Deployment is scaled: API/orchestrator, task workers, inference server, or combination. First HPA experiment should scale one layer only. Scaling API Pods does not automatically add GPU inference capacity.
