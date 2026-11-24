# 09 — Experiment Methodology

## Sequence
1. Validate generator/workflow semantics locally.
2. Deploy to Kubernetes.
3. Run fixed-replica baseline.
4. Run reactive HPA baseline under same scenarios.
5. Compare by workflow pattern and offered load.
6. Consider workflow-aware signals only if data supports the need.

## Baselines
- **B0 Fixed replicas:** fixed replica count, no horizontal scaling during run.
- **B1 Reactive:** HPA using a documented metric (CPU candidate). Record target, threshold, min/max, stabilization, and metrics source.
- **B2 Optional workflow-aware:** only after B0/B1 are reliable. Candidate signals: queue depth, active workflows, estimated remaining tasks/LLM calls, workflow type. No policy is selected yet.

## Application metrics
Workflow duration, queue wait, throughput, success/failure/timeout/rejection, p50/p95/p99 when sample size supports them, task duration by type, active workflows/tasks, LLM TTFT/latency/tokens when real inference is enabled.

## Infrastructure metrics
CPU/memory usage and requests/limits; replica counts; HPA current/desired; Pod pending/scheduling; Pod creation-to-Ready; node utilization; restarts.

## Timing decomposition
Measure offered arrival, generator dispatch lag, API queue wait, workflow execution, metric observation delay, HPA decision delay, Pod scheduling/startup/readiness, and time until new capacity serves useful work.

## Controls
Same scenario/seed, generator placement, arrival schedule, resource requests/limits, application/model version, warm-up/cooldown, and documented cluster background load. Repeat runs and retain raw data. Flag generator saturation or network faults.

## Validity threats
Generator bottleneck; shared LLM/GPU bottleneck; cluster background load; variable image pull/startup; synthetic delay misrepresented as inference; HPA sampling/control delay unobserved; state lost on restart; unstable tail percentiles from small sample sizes.

## SLO
No final SLO target is confirmed. Define targets before evaluating results; do not invent thresholds after seeing results.
