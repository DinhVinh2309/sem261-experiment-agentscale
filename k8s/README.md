# Kubernetes deployment workspace

Create deployment manifests here only after auditing the actual cluster and application image.

Recommended initial resources:
- Namespace `llm-agent-experiments`
- Workflow API Deployment + ClusterIP Service
- ConfigMap for non-secret settings
- Secret template (never commit real values)
- Optional workload-generator Job
- ServiceMonitor only if the installed Prometheus Operator supports it

Start with one API replica if state is in memory. Add HPA only after confirming the metrics pipeline and scaling target. Do not apply generic templates blindly to the user's cluster.
