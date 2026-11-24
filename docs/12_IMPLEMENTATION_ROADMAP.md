# 12 — Stage 3 Implementation Roadmap

## M0 — Repository and cluster audit
Inspect source tree, git status, dependencies, Dockerfiles/manifests. Audit node roles/version/CNI/runtime/capacity and Rancher monitoring. **Exit:** actual baseline/gap report; no destructive changes.

## M1 — Skeleton and scenario validation
Python package, Pydantic models, safe YAML loader, schema version, CLI validate, tests, sequential example. **Exit:** normalized config output.

## M2 — Workload generator
Open-loop constant rate; IDs; max in-flight/backpressure; HTTPX dispatch; scheduled/actual arrival records. **Exit:** controlled requests to stub API.

## M3 — API and sequential workflow
FastAPI health/readiness, submit/status/result, bounded queue, synthetic adapter, sequential orchestrator, JSONL. **Exit:** end-to-end local scenario.

## M4 — Parallel
Bounded fan-out/fan-in, branch timing, cancellation/timeout tests. **Exit:** concurrent branches and correct join.

## M5 — Dynamic
Deterministic conditions, bounded loops/retries, branch records. **Exit:** reproducible branch paths.

## M6 — Metrics and experiment runner
Prometheus metrics, manifest/result directory, summary calculations. **Exit:** complete artifacts per run.

## M7 — Container and Kubernetes
Pinned image, API deployment, probes, requests/limits, ConfigMap/Secret, generator placement, cluster smoke tests. **Exit:** Stage 3 platform on 1 master + 3 workers.

## M8 — Real LLM adapter (conditional)
Choose backend/model based on hardware; capture tokens/latency; retain synthetic mode. **Exit:** real inference mode distinguishable and reproducible.

## M9 — Autoscaling preparation
Choose one target; fixed baseline; verify metrics pipeline; configure HPA; record control and Pod readiness timing. **Exit:** ready for comparative experiments.

Do not jump to M8/M9 while M1–M6 are unstable. Valid load generation and telemetry are prerequisites.
