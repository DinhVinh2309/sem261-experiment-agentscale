# 06 — Execution and API Design

## Workflow lifecycle
Suggested states: `accepted -> queued -> running -> succeeded | failed | timed_out | cancelled`. Requests refused before acceptance are `rejected`. Validate state transitions.

## Task executor interface
Use a replaceable abstraction conceptually like:
```python
class TaskExecutor(Protocol):
    async def execute(self, task: TaskDefinition, context: WorkflowContext) -> TaskResult:
        ...
```
Workflow graph semantics must not depend on synthetic versus real LLM execution.

## Execution rules
- **Sequential:** execute in declared order; pass typed outputs through context; apply explicit failure policy.
- **Parallel:** use `asyncio.TaskGroup` or equivalent structured concurrency; bound fan-out with semaphore; await required branches; cancel and record remaining branches on timeout.
- **Dynamic:** deterministic condition evaluator for synthetic mode; record branch; bound loops using `max_iterations` and a global deadline.
- **Retries:** only retry declared retryable failures; bounded attempts/backoff; record each attempt; do not retry non-idempotent tool actions without idempotency.

## Proposed API base path
`/api/v1`

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/healthz` | Liveness |
| GET | `/readyz` | Readiness |
| GET | `/metrics` | Prometheus exposition |
| POST | `/api/v1/workflows` | Submit workflow |
| GET | `/api/v1/workflows/{workflow_id}` | Status/metadata |
| GET | `/api/v1/workflows/{workflow_id}/result` | Final result |

### Submit request
```json
{
  "experiment_id": "exp-001",
  "workflow_id": "wf-000001",
  "workflow_type": "sequential",
  "execution_mode": "synthetic",
  "seed": 42,
  "input": {"profile": "medium"},
  "timeout_seconds": 120
}
```
Return `202 Accepted` and workflow ID for asynchronous queued execution. Define idempotency for duplicate IDs.

## Backpressure
Use a bounded queue. `wait` waits up to a configured maximum; `reject` returns 429/503 and is counted. Never create an unbounded in-memory queue.

## State caveat
In-memory state is acceptable for one API replica in the first vertical slice. It is lost on restart and unsafe across replicas. Before multiple API replicas, add shared durable state or stateless task dispatch.
