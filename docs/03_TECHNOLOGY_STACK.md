# 03 — Technology Stack and Decisions

| Layer | Recommended choice | Purpose | Status |
|---|---|---|---|
| Language | Python 3.12+ | Generator/orchestrator/runner | Proposed |
| API | FastAPI + Uvicorn | Async HTTP/OpenAPI | Proposed |
| Validation | Pydantic v2 | Typed models | Proposed |
| YAML | PyYAML `safe_load` | Scenario files | Proposed |
| Async | asyncio / TaskGroup | Concurrent execution | Proposed |
| HTTP client | HTTPX AsyncClient | Generator/API | Proposed |
| Metrics | prometheus-client | App metrics | Proposed |
| Monitoring | Prometheus + Grafana | Time series/dashboards | Verify cluster |
| Resource metrics | Metrics Server | HPA CPU/memory | Verify cluster |
| Container | Docker/OCI | Reproducible image | Proposed |
| Orchestration | Kubernetes | Experiment runtime | Confirmed |
| Management | Rancher | Cluster management | Installed server confirmed; version TBD |
| Testing | pytest + pytest-asyncio | Unit/integration tests | Proposed |
| Results | JSONL + CSV + JSON manifest | Raw data/summaries | Proposed |
| LLM serving | vLLM/SGLang/external API | Real inference | TBD |

## Implementation guidance
Start as a modular Python application with clear modules: `config`, `generator`, `api`, `orchestrator`, `executors`, `telemetry`, `runner`. Run generator and API as separate processes when needed. This avoids premature microservices.

Use existing repo conventions and lock dependencies. Avoid adding Kafka, Redis, PostgreSQL, Celery, LangChain, or a workflow framework without a concrete requirement.

### State
In-memory workflow state is acceptable only for a single API replica and short experiments. It is lost on restart and unsafe across replicas. Before scaling API replicas above one, introduce shared durable state or stateless dispatch.

### Generator placement
Host-side generator isolates load generation from cluster CPU; Kubernetes Job is more reproducible/in-cluster but competes for resources. Record placement. Start host-side unless networking or operational constraints dictate otherwise.

### Execution modes
Synthetic first to validate orchestration and scaling measurement. Add real LLM only after telemetry is stable. Never report synthetic timing as actual GPU inference performance.

### Configuration and secrets
YAML for scenarios; environment variables for runtime; ConfigMaps for non-secret config; Secrets for credentials. Never commit secrets.
