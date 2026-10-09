---
name: observe-core-job-lifecycle
description: Guide TplQueue consumers observing IQ.Subscribe and observable Job events, including asynchronous snapshots, OnNext versus OnError, slow observers, ordering, WaitAsync limits, and optional execution-channel metadata.
---

# Observe Core Job lifecycle

Read applicable repository instructions, current `IQ`, `IJobEvent`, and `IJobExecutionEvent` contracts, maintained public documentation, and relevant consumer observations. Follow the target version if the baseline evolves; flag contradictions. Do not expose private observer implementations, buffering, dispatcher structure, source, or tests.

## Subscribe to lifecycle snapshots

Use `IQ.Subscribe` / `IObservable<IJobEvent>`. Describe the model as the Observer pattern with asynchronous event dispatch: queue orchestration and Job execution publish lifecycle snapshots for asynchronous delivery. Observer callbacks are outside the Job body execution path. Manage the returned subscription according to the public subscription contract and the consumer's lifetime.

Treat `IJobEvent` as an immutable lifecycle snapshot at publication time, rather than a live view of mutable Job state. Current public statuses include `Enqueued`, `Started`, `Running`, `Successed`, `Failed`, `Canceled`, and `RootSuccessed`; preserve these spellings. This list does not establish event ordering.

| Situation | Consumer interpretation |
| --- | --- |
| Ordinary Job execution failure | `OnNext` receives an `IJobEvent` with `Failed` status. |
| Fatal dispatcher failure | `OnError` reports a queue-level failure that can make the queue unusable, according to `IQ`. |
| Subscriber throws | An observation-processing failure, not a Job execution failure. |

Observers should handle their own processing failures. Do not turn every failed Job into Observable `OnError`, or assume a subscriber exception changes the Job outcome.

## Keep observation responsive

Return from callbacks as quickly as practical. Avoid inline blocking network/database access, file I/O, long CPU work, long locks, or synchronous waits over asynchronous operations. For expensive processing, capture the required immutable event data, hand it to a consumer-owned asynchronous processing mechanism, and return. Do not prescribe a buffering library without an application requirement; avoid `async void` as a general observer-processing strategy.

Asynchronous delivery means a slow subscriber should not directly block the executing Job. Delivery capacity is shared, however: slow callbacks can delay later notifications, other subscribers, and observer-driven secondary behavior such as CacheQ terminal acknowledgment. Treat this as a throughput and latency concern; do not promise unlimited delivery capacity.

Concurrent execution can publish events concurrently. Callback arrival order is not a global scheduling-order guarantee. Correlate events by Job identity, status, timestamps, and other public metadata; do not reconstruct execution solely from callback order.

`WaitAsync` does not drain observer delivery or consumer-owned processing. If completion of observation matters, identify a separate consumer-owned completion condition. Use [CacheQ semantics](../use-core-cacheq-semantics/SKILL.md) for acknowledgment requirements; a completed Job notification alone does not prove every downstream cache transition has finished.

## Interpret execution channels

`IJobExecutionEvent` is optional additive metadata over `IJobEvent`. Consumers must support events without that interface and nullable `ExecutionChannel` values.

- A non-null channel identifies queue-local execution capacity. It is diagnostic metadata, not an OS thread ID, root/group identity, globally unique identity, or scheduling command.
- Pre-execution events may have no assigned channel.
- Once capacity is assigned, the channel remains stable across retries while that capacity is retained.
- Terminal snapshots may retain the assigned channel captured before capacity is released.

Route changes to the execution-channel contract itself to `tplqueue-abstractions` when available.

## Report

State subscription/lifecycle behavior, `OnNext` versus `OnError`, ordering assumptions, callback-performance implications, execution-channel interpretation, and whether `WaitAsync` is sufficient for the consumer's actual requirement.
