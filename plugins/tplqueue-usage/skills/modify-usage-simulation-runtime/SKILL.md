---
name: modify-usage-simulation-runtime
description: Modify TplQueue.Usage simulation workflows, scheduled delivery, host lifecycle, graph tracking, and shared queue runtime while preserving workload ownership and observer-visible semantics.
---

# Modify Usage simulation runtime

Complement engineering's `implement-feature`, `review-concurrency`, `testing-dotnet`, and `validate-dotnet-change`, and [review-usage-sample-architecture](../review-usage-sample-architecture/SKILL.md), when available and relevant. Keep generic implementation and concurrency procedures in engineering.

## Establish the current runtime

Read applicable parent instructions, the target root `AGENTS.md` or `Agents.md` and delegated/scoped sample instructions, the maintained Blazor/sample architecture guide, workflow contracts and implementations, runtime implementation, and affected tests. Resolve the guide from the target instructions; its current path is `docs/architecture/blazor-consumer-sample.md` relative to Usage.

The target source, tests, and maintained guide remain authoritative. Surface conflicting evidence before dependent edits. Preserve the following model unless the explicit task deliberately changes it.

## Preserve scheduling and lifecycle ownership

The current ETL and SingleJob workflows implement `ISimulationWorkflow` through `Workflows/ScheduledWorkflow.cs`. Each workflow owns one timer that attempts submission to all three queues. The baseline is a one-second initial offset and three-second interval; follow source if those values deliberately change later. Do not add independent timers merely for convenience.

| Owner or contract | Lifecycle responsibility |
| --- | --- |
| Workflow `StopAsync` | Timer cleanup. |
| `ISimulationWorkflow` and `ISimulationService` | Neither is `IDisposable`. |
| Hosted service | Awaits `StopAsync`. |
| Runtime disposal | Queue and subscription cleanup. |

Do not duplicate lifecycle ownership across the hosted service, workflows, and runtime. The host starts its workload once, independently of browsers. Browser connections must not create, restart, or multiply workloads. Stopping new arrivals does not cancel accepted jobs unless the explicit task changes that behavior.

## Preserve queue and graph identity

The singleton `IEtlQueueRuntime` owns/captures the queues used for submission. Monitoring and observers must use these same instances; do not resolve replacement wrappers solely for observation.

Capture simulation graph membership before enqueue through `ISimulationGraphCatalog`. Preserve every `rootJobIds` membership for shared jobs; a root ID identifies the current one-root run.

Membership is composition metadata, not proof of queue acceptance, queue ownership, lifecycle state, execution channel, or successful execution. Keep actual dependency relationships, including FIFO ordering edges where represented, independent of membership metadata.

## Validate the changed behavior

Apply engineering's `review-concurrency` when available for changes to timer scheduling, submission overlap, stopping, disposal, queue ownership, graph tracking, or shared runtime state. Inspect the affected ownership and race boundaries even when that companion skill is unavailable.

Update tests for the lifecycle or simulation behavior actually changed. Preserve existing tests unless demonstrably obsolete under an explicitly changed contract. Keep implementation within the authorized repository scope; identify required dependency changes separately.

## Report

State the workflow changed, timer/lifecycle impact, host ownership impact, queue-runtime impact, graph-catalog impact, concurrency implications, tests and validation, and related consumer/UI impact.
