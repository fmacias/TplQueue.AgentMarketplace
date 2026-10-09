---
name: configure-core-retry-behavior
description: Explain or review TplQueue.Core root-versus-queue retry selection, explicit NoRetry, null policy results, Job identity, RetryCount, cancellation, execution channels, and policy-factory failures. Adapter named policy registration and DI remain outside this workflow.
---

# Configure Core retry behavior

Complement engineering's `review-concurrency`, `review-public-api`, and `testing-dotnet` when available and relevant. Read applicable repository instructions, current public retry/Job/queue contracts, maintained public documentation, and consumer-visible evidence. The target version remains authoritative; surface contradictions. Authorized private inspection may verify selection behavior, but do not expose the private selection implementation.

## Determine the effective policy

Describe `IRetryPolicy` as a Retry Policy Strategy. Factory delegates supply the appropriate policy instance for execution. Preserve these distinct consumer states:

| Root policy | Queue policy | Effective behavior |
| --- | --- | --- |
| Explicit factory produces a policy | Any | Effective root policy overrides the queue policy. |
| Unspecified | Available | Root inherits the queue policy. |
| Explicit NoRetry | Any | NoRetry overrides the queue policy; it is not an unspecified root. |
| Explicit factory returns no policy | Available | Current behavior falls back to the queue policy. |
| No effective root policy | No effective queue policy | Current behavior falls back to NoRetry. |

A factory returning no policy and a factory throwing are different cases. Do not describe a thrown exception as a null-policy fallback.

Policy names, options, lookup, DI builders, and concrete policy configuration belong to `tplqueue-adapter`. Identify that dependency when relevant without duplicating its configuration workflow.

## Preserve execution meaning

- Retry remains execution of the same Job with the same Job identity, rather than creating an independent Job for each attempt.
- `IJobEvent.RetryCount` exposes retry information to observers. Interpret it using the current policy/event contract; do not invent an extra attempt-number convention or a new identity from that count.
- Once queue capacity is assigned, retries retain the same queue-local execution channel while that capacity remains held. A retry does not imply a different thread or independent channel. See [Job observation](../observe-core-job-lifecycle/SKILL.md).
- Policies receive the Job execution `CancellationToken`. Preserve consumer cancellation unless the specific policy contract explicitly permits different behavior.

## Distinguish failure modes

Separate operation failure handled within policy execution, final Job failure after policy execution, and factory/configuration failure that prevents execution. Final ordinary Job failure is represented through lifecycle `OnNext` with `Failed` status.

A retry-policy factory failure can instead become a fatal dispatcher failure: `WaitAsync` may fault, the queue may reject further work, and observer `OnError` may report the failure. Do not present this as ordinary retry exhaustion without evidence. See [queue lifecycle](../use-core-queue-lifecycle/SKILL.md) for the unusable-queue boundary.

## Report

State root and queue policies, effective precedence, explicit NoRetry behavior, `RetryCount` implications, execution-channel and cancellation implications, ordinary versus fatal failure semantics, and relevant Adapter configuration impact.
