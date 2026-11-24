# Codex Instructions — Workload-Aware Autoscaling for LLM Agents on Kubernetes

## Mission
Build the Stage 3 experimental platform for **Workload-Aware Autoscaling for LLM Agents on Kubernetes**: scenario configuration, workload generation, workflow execution, telemetry, experiment orchestration, and Kubernetes deployment.

## Read first
Read `docs/00_PROJECT_BRIEF.md` through `docs/15_OPEN_QUESTIONS.md`. Prioritize Stage 3 requirements, architecture, technology stack, workload model, scenario configuration, execution/API, telemetry, Kubernetes/Rancher, experiment methodology, roadmap, and current environment.

## Research invariants
1. Current title: **Workload-Aware Autoscaling for LLM Agents on Kubernetes**.
2. Build a valid, reproducible measurement platform before autoscaling comparisons.
3. A workflow is one logical task graph; a workload is the aggregate stream of workflow arrivals and demand.
4. Planned workflow types: sequential, parallel fan-out/fan-in, and dynamic/conditional.
5. The Workload Generator controls arrivals and offered load. The Orchestrator controls internal workflow steps and dependencies.
6. YAML is configuration; it does not generate traffic by itself.
7. First evaluate fixed replicas and conventional reactive autoscaling. A novel autoscaling algorithm is not a precondition.
8. Keep Kubernetes Pod autoscaling distinct from LLM serving-engine scheduling. MARS is agent-centric scheduling literature, not a Kubernetes HPA implementation.
9. Never invent implementation status, results, paper metadata, hardware/GPU availability, or cluster details.
10. Mark information CONFIRMED, PROPOSED, or TBD. Ask before changing research scope.

## Latest confirmed infrastructure
The user reports **1 Kubernetes master/control-plane node, 3 worker nodes, and 1 Rancher Server**. Node names/IPs, Kubernetes version, CNI, runtime, hardware, GPU availability, monitoring add-ons, and Rancher version are unknown. Treat Rancher Server as separate management infrastructure unless confirmed otherwise.

## Preferred stack if the repository has no established choices
Python 3.12+, FastAPI/Uvicorn, Pydantic v2, PyYAML safe loading, asyncio/TaskGroup, HTTPX, prometheus-client, Docker/OCI, Kubernetes Deployments/Services/Jobs/ConfigMaps/Secrets/HPA, Prometheus/Grafana, Metrics Server, pytest/pytest-asyncio, JSONL/CSV. These are recommendations, not claims of installation.

## Engineering rules
- Inspect repository, git status, dependencies, and manifests before editing.
- Do not overwrite user work or perform destructive cluster actions without approval.
- Implement small vertical slices with tests.
- Use bounded queues/concurrency, explicit timeouts, bounded retries, graceful shutdown, and idempotency where needed.
- Never use unsafe YAML loading or commit secrets.
- Do not use unique workflow IDs as Prometheus labels; store them in logs/JSONL/traces.
- Record scenario snapshot, seed, git commit, software versions, run timestamps, resource requests/limits, and generator placement.
- Update current environment, decision log, and open questions after material changes.

## First action in a new Codex session
Read relevant docs; inspect repository and git status; perform read-only cluster audit; report actual components and gaps; recommend one small next milestone. Do not make broad changes before agreement.
