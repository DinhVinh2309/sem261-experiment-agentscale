# 15 — Open Questions

## Repository
- Existing source tree, language, dependencies, package manager?
- Existing code to extend?
- Current branch and uncommitted changes?

## Cluster
- Node names/IPs and CPU/RAM/disk?
- Kubernetes version/distribution, CNI, runtime?
- Metrics Server healthy?
- Prometheus/Grafana already available through Rancher monitoring?
- StorageClass?
- GPU/device plugin?
- Registry access for worker nodes?

## LLM serving
- GPU available on any worker?
- Which model and engine?
- In-cluster or external inference?
- Should initial experiments use synthetic tasks only?

## Application
- Generator on workstation or as Kubernetes Job?
- One API replica initially?
- When will shared workflow state be needed?
- Queue capacity and overload policy?

## Experiment design
- Arrival rates, count, duration, concurrency?
- Warm-up/drain?
- Repetitions?
- SLOs?
- First HPA target Deployment?

## Recommended immediate next step
Ask Codex to perform a read-only repository and cluster audit, then confirm stack and implement M1 (scenario loader/validator) before generator development.
