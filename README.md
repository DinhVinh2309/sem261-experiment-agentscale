# Project Context Package — Codex / VS Code

This bundle provides repository-ready context for building the Stage 3 experimental platform for **Workload-Aware Autoscaling for LLM Agents on Kubernetes**.

## Contents
- `AGENTS.md`: Codex instructions and research invariants.
- `docs/`: research scope, engineering requirements, architecture, stack, workload model, APIs, telemetry, Kubernetes/Rancher environment, experiments, tests, roadmap, decisions, status, and open questions.
- `examples/scenarios/`: sequential, parallel burst, and dynamic YAML examples.
- `k8s/README.md`: deployment planning notes.

## Install into the research repository
1. Extract this archive.
2. Copy `AGENTS.md` into the root of the existing repository.
3. Merge `docs/`, `examples/`, and `k8s/` into the repository; preserve existing files.
4. Open the repository root in VS Code and start Codex.
5. Ask Codex to read `AGENTS.md`, audit the codebase and cluster read-only, and report discrepancies before editing.

## Important
The latest confirmed cluster topology is 1 master/control-plane + 3 workers + 1 separate Rancher Server. Node details and installed add-ons are unknown. Proposed architecture/technology is marked as recommended or TBD, not as already implemented. This package is reconstructed from available project context, not a literal export of every chat or a live cluster audit.
