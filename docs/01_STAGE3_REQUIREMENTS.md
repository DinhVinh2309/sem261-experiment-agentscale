# 01 — Stage 3 Requirements

## Objective
Build the platform required to run repeatable sequential, parallel, and dynamic workflow experiments and collect data for later autoscaling evaluation.

## Functional requirements
- **Scenario management:** safe YAML load; schema/version validation; typed fields, ranges, enums, cross-field rules; normalized snapshot.
- **Workload generation:** finite count and/or duration; open-loop constant rate first; burst next; Poisson optional; max in-flight; warm-up, stop-arrivals, drain, cooldown; planned and actual dispatch timestamps; explicit backpressure.
- **Workflow execution:** sequential dependencies; bounded parallel fan-out/fan-in; dynamic branches with deterministic synthetic conditions and bounded loops; IDs propagated; per-step timing/status/errors.
- **Task execution:** synthetic adapter first; common interface for real LLM adapter; timeout and bounded retry; explicit synthetic/real mode; token estimates or actual token counts.
- **API:** health/readiness; submit; status/result; Prometheus metrics; structured errors and capacity handling.
- **Observability:** structured logs; workflow/task metrics; Kubernetes Pod/Deployment/HPA metrics; no high-cardinality IDs as Prometheus labels.
- **Reproducibility:** scenario snapshot, seed, run index, git commit, app version, environment, timestamps, resource settings; JSONL/CSV raw records; record every terminal/failure state.
- **Kubernetes:** container image; Deployment/Service; generator external or Job; ConfigMaps/Secrets; requests/limits; probes; graceful shutdown; configurable replicas.

## Non-functional requirements
Reproducible arrival schedules; bounded memory/concurrency; no silent loss; testable without live LLM/cluster; portable local/container/Kubernetes configuration; no secrets in source control.

## Acceptance criteria
1. YAML loads and validates.
2. Generator submits intended workflow count/rate within measured tolerance.
3. Sequential workflow runs end-to-end.
4. Parallel branches overlap and join correctly.
5. Dynamic branch is recorded and loop-bounded.
6. Workflow/task records correlate by IDs.
7. Metrics endpoint is scrapeable.
8. Each run creates a self-contained result directory.
9. Container deploys to the user's cluster.
10. Same scenario can be repeated with recorded parameters.
