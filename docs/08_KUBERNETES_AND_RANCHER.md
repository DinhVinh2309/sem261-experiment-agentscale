# 08 — Kubernetes and Rancher Environment

## Latest confirmed topology
The user confirms:
- **1 master/control-plane node**
- **3 worker nodes**
- **1 Rancher Server**

The Rancher Server is described separately. Do not count it as one of the four Kubernetes nodes unless the user confirms it is joined to the cluster. This topology supersedes earlier planning notes about three VMs.

## Unknown details
Node names/IPs and CPU/RAM/disk; OS; Kubernetes version/distribution; container runtime; CNI; storage class; Metrics Server; Prometheus/Grafana; Rancher version and management mode; GPU/device plugin; registry access are not yet known.

## Read-only cluster audit
Run using the intended kubeconfig:
```bash
kubectl get nodes -o wide
kubectl version
kubectl get pods -A
kubectl get svc -A
kubectl get storageclass
kubectl top nodes
kubectl top pods -A
kubectl get apiservices
```
If `kubectl top` fails, record the error; do not assume Metrics Server exists. Use `kubectl describe nodes` for allocatable resources and conditions. Never copy kubeconfig/token content into docs.

## Role separation
- Control plane manages cluster state.
- Workers host application Pods.
- Rancher Server provides cluster management UI/API; it is not the Kubernetes scheduler or autoscaler.
- HPA changes desired replica count.
- Kubernetes scheduler places newly created Pods on suitable nodes; it does not decide Deployment replica count.

## Deployment recommendations
- Namespace: `llm-agent-experiments`.
- API/orchestrator: Deployment + ClusterIP Service.
- Generator: initially workstation process or a Job per experiment.
- ConfigMap for non-secret config; Secret for credentials.
- Set requests/limits and readiness/liveness probes.
- Use pinned image tags, not `latest`.
- Start with one API replica if state is in memory.
- Reuse existing Rancher/cluster monitoring if present; avoid duplicate Prometheus/Grafana installs.

## HPA prerequisites
Metrics Server for CPU/memory or custom metrics adapter; CPU requests configured for CPU utilization target; explicit target Deployment/min/max/metric/stabilization settings; capture HPA status/events and Pod readiness. Do not choose replica limits or CPU thresholds before auditing node capacity.
