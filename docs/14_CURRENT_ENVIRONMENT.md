# 14 — Current Environment Snapshot

**Last updated:** 2026-11-24. Source: user statement and prior project discussions; not a live audit.

## Confirmed topology
| Component | Count | Status |
|---|---:|---|
| Kubernetes master/control-plane | 1 | User confirms installed |
| Kubernetes worker nodes | 3 | User confirms installed |
| Rancher Server | 1 | User confirms installed |

Rancher Server is described separately. Do not assume it is a cluster worker/control-plane node.

## Unknown
Node names/IPs and CPU/RAM/disk; OS; Kubernetes version/distribution; container runtime; CNI; storage class; Metrics Server; Prometheus/Grafana; Rancher version/mode; GPU/device plugin; registry access.

## Earlier hands-on history
User practiced Kubernetes on Ubuntu 24.04 VMs using VMware/bridged networking, deployed nginx with three replicas and exposed it via NodePort. Earlier planning used three VMs but the latest topology is 1 master + 3 workers + Rancher Server. A previous Rancher Docker attempt logged a `--privileged` requirement outside Kubernetes; latest user update confirms Rancher Server is now installed, so do not assume that old issue remains.

## Read-only audit commands
```bash
kubectl get nodes -o wide
kubectl version
kubectl get pods -A
kubectl get svc -A
kubectl get storageclass
kubectl top nodes
kubectl top pods -A
kubectl get apiservices
kubectl describe nodes
```
If `kubectl top` fails, record it; do not assume Metrics Server is installed. Never copy kubeconfig contents or tokens into docs.

## Update policy
Replace unknowns only with verified values. Record date and command/result summary. Never put secrets here.
