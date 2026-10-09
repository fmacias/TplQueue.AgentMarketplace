---
name: use-core-queue-lifecycle
description: Explain or review TplQueue.Core ParallelQ, FifoQ, and CacheQ consumer lifecycle semantics, including bounded execution, Pause, ResumePolling, WaitAsync, cancellation, fatal queue state, and disposal.
---

# Use Core queue lifecycle

Complement engineering's `review-concurrency`, `testing-dotnet`, and `validate-dotnet-change` when available and relevant. Read applicable repository instructions, public queue contracts, maintained public documentation, and relevant observable behavior. The target version remains authoritative; flag conflicting claims. Use authorized private inspection only to verify public semantics, without publishing implementation details.

## Select the queue model

| Queue | Consumer model |
| --- | --- |
| ParallelQ | Bounded-concurrency dispatcher |
| FifoQ | Strict FIFO / serial dispatcher |
| CacheQ | Cache-backed dispatcher integration |

Use current Job terminology; reserve TaskRunner for historical migration context. `MaxParallelism` describes queue execution capacity, not an operating-system thread count. Capacity and thread identity are separate concepts.

## Explain lifecycle boundaries

- `Pause` stops polling/admission of additional buffered work according to the queue contract. It does not cancel already executing work or promise immediate interruption of an in-flight admission.
- `ResumePolling` resumes queue polling. Do not introduce restart or recreation semantics that the public contract does not define.
- `WaitAsync` waits for queue-owned buffered/running work to finalize according to `IQ`. It does not drain observer callbacks, consumer-owned asynchronous processing, or CacheQ persistence/acknowledgments. Successful completion does not establish that every Job succeeded or that external side effects finished.
- The token associated with queued work is execution-cancellation input. Cancellation is separate from disposal; do not promise immediate interruption beyond the contract and observable behavior.
- Ordinary Job failure is represented by Job lifecycle events. Fatal dispatcher failure can make the queue unusable, fault `WaitAsync`, and cause later operations to fail. Use [Job observation](../observe-core-job-lifecycle/SKILL.md) for `OnNext` versus `OnError`.
- After disposal, treat the queue as unavailable. Disposal is not a substitute for `WaitAsync` or external acknowledgment.

For CacheQ, apply [CacheQ semantics](../use-core-cacheq-semantics/SKILL.md) as well: underlying queue completion does not prove that all cache-backed work or terminal cache transitions have completed. If a consumer needs a stronger completion condition, identify the separately observable condition and its owner instead of attributing it to `WaitAsync`.

## Report

State queue type, execution capacity, pause/resume implications, what `WaitAsync` establishes, cancellation and disposal implications, and asynchronous work remaining outside the queue boundary.
