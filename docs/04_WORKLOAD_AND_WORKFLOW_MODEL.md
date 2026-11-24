# 04 — Workload and Workflow Model

## Definitions
- **Workflow:** one logical end-to-end task graph.
- **Workload:** aggregate stream of workflows and demand over time: arrival rate, concurrency, workflow mix, task count, tokens, execution time.
- **Scenario:** versioned configuration of an experiment.
- **Offered load:** work the generator attempts to submit.
- **Throughput:** completed workflows per unit time; not equivalent to offered arrival rate.
- **Concurrency:** number of workflows/tasks in progress simultaneously.

## Workload dimensions
Workflow type/mix; open/closed-loop; arrival model/rate; total count/duration; max in-flight; warm-up/drain/cooldown; graph shape/fan-out; synthetic/LLM mode; token profile; timeout/retry; seed; overload policy.

## Arrival models
- **Open-loop:** arrivals independent of completion; preferred for controlled offered-load tests.
- `constant_rate`: fixed interval 1/rate; implement first.
- `burst`: rate changes by configured phases; implement next.
- `poisson`: exponential inter-arrival times; optional.
- **Closed-loop:** next request follows prior completion; offered load depends on response time. Keep separate from open-loop results.

## Workflow patterns
### Sequential
`analyze_input -> extract_facts -> evaluate -> compose_result`
Each step waits for its predecessor. Multiple workflows can overlap.

### Parallel fan-out/fan-in
`prepare -> [analyze_a, analyze_b, analyze_c, analyze_d] -> aggregate`
Branches start concurrently; first implementation waits for all required branches. Record branch times and join wait.

### Dynamic / conditional
`classify -> simple_path OR complex_path -> finalize`
Complex path may have bounded revise/evaluate loop. Record decision, reason/category, iterations, and executed steps. Use deterministic seeded conditions in synthetic experiments; do not let an unconstrained LLM arbitrarily change workload size in controlled tests.

## Task profile
Task ID/type/mode; synthetic delay or controlled CPU work; estimated input/output tokens; timeout; retries; dependencies. Synthetic delay is not equivalent to CPU/GPU inference.

## Intensity
Do not label load low/medium/high without numeric arrival rate, concurrency, workflow mix, task count, and token profile. Calibrate after cluster capacity and baseline behavior are measured.
