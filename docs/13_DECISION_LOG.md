# 13 — Decision Log

## Confirmed
- **D-001 Topic:** Workload-Aware Autoscaling for LLM Agents on Kubernetes.
- **D-002 Evidence-first:** test existing reactive autoscaling before deciding whether a workflow-aware policy is needed. A novel algorithm is not mandatory by assumption.
- **D-003 Boundary:** primary focus is Kubernetes resource management/autoscaling; LLM serving is an underlying dependency.
- **D-004 Individual project:** prioritize feasible and measurable scope.
- **D-005 Workflow patterns:** sequential, parallel fan-out/fan-in, dynamic/conditional.
- **D-006 Cluster topology:** 1 master/control-plane, 3 workers, 1 separate Rancher Server. Node details remain TBD.

## Proposed, not confirmed
Python/FastAPI/Pydantic/asyncio/HTTPX; Prometheus/Grafana and Metrics Server; host-side generator initially; synthetic execution before real LLM; in-memory state for one replica only; CPU-based HPA as first reactive baseline.

## Historical direction
An earlier idea concerned ML-based adaptive priority scheduling for CI/CD pipelines. The advisor-guided direction shifted to LLM workloads on Kubernetes. Do not revive the old topic unless asked.

## Change control
For major changes, explain motivation, evidence, cost, scope impact, and alternatives; ask before treating a proposal as a decision.
