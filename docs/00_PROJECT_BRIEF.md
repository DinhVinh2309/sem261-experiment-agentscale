# 00 — Project Brief

## Research title
**Workload-Aware Autoscaling for LLM Agents on Kubernetes**

## Repository purpose
Build a reproducible experimental platform to generate, execute, observe, and analyze agentic workflow workloads on Kubernetes. Immediate focus: **Stage 3 — workload modeling and platform implementation**.

## Motivation and research question
An agentic request can expand into multiple dependent LLM calls, tool operations, parallel branches, conditional paths, and intermediate processing. Resource metrics such as CPU/memory may not directly reveal the amount or structure of pending workflow work.

**Research question:** How effectively do conventional Kubernetes reactive autoscaling mechanisms respond to changing resource demands from different LLM-agent workflow patterns, and can workflow-level information help characterize or improve scaling behavior?

## Immediate system objectives
1. Versioned scenario definition and validation.
2. Workload generation with controllable arrivals, rate, duration/count, and concurrency.
3. Sequential, parallel, and dynamic workflow orchestration.
4. Synthetic task execution for controlled experiments.
5. Extensible real-LLM adapter.
6. API contracts and workflow state handling.
7. Application and Kubernetes observability.
8. Experiment runner and reproducible result capture.
9. Containerization and Kubernetes deployment.
10. A stable platform for fixed-replica and HPA comparisons.

## Conceptual boundaries
- Scenario YAML declares experiment parameters.
- Workload Generator emits workflow submissions over time.
- Workflow API accepts submissions.
- Orchestrator executes workflow graph.
- Task Executor runs individual tasks.
- LLM Serving performs inference if real-model mode is enabled.
- Kubernetes schedules Pods and manages scaling.
- Monitoring collects application and infrastructure signals.

The platform is a research prototype, not a production multi-tenant agent platform. Avoid unnecessary brokers, databases, service meshes, and workflow frameworks.
