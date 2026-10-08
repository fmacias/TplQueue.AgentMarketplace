---
name: modify-adapter-cacheq-lifecycle
description: Modify TplQueue.Adapter CacheQ observation, terminal cache transitions, queue completion, Observer delivery, acknowledgment timing, private subscription ownership, or disposal semantics.
---

# Modify Adapter CacheQ lifecycle

Complement `modify-adapter-observer-flow` and engineering's `review-concurrency`, `implement-feature`, `testing-dotnet`, and `validate-dotnet-change` when available and relevant.

## Establish lifecycle contracts

Read applicable parent instructions, target root `AGENTS.md`, nearest cache package `AGENTS.md`, CacheQ/cache contracts, relevant observer contracts and implementation, affected source/tests, and applicable documentation. The target repository remains authoritative; surface conflicts before dependent changes.

Preserve this model unless the explicit task deliberately changes it:

- CacheQ owns a private cache observer and that observer's subscription to the supplied queue.
- Terminal cache transitions run through the private cache observer. Enqueue and dispatch do not apply terminal transitions inline.
- `WaitAsync` waits for underlying queue work, not Observer delivery or cache acknowledgment. Do not accidentally strengthen it into a cache synchronization/acknowledgment barrier.
- Slow subscribers on the shared observer hub can delay cache updates.
- `CacheQ.Dispose` unsubscribes its observer, so queued notifications may remain unapplied. Disposal is not an acknowledgment barrier and does not guarantee all Observer/cache work has been applied.

Do not bypass the Observer path to force synchronous cache state.

## Change and validate

1. Trace affected timing and races across enqueue, dispatch, successful completion, failure, cancellation, retry, terminal cache transition, Observer delivery, and disposal.
2. When changing the shared notification architecture, also apply [modify-adapter-observer-flow](../modify-adapter-observer-flow/SKILL.md).
3. Preserve package boundaries and production `netstandard2.0` compatibility unless the explicit task deliberately changes the framework policy.
4. Add/update applicable tests for underlying queue completion, asynchronous terminal cache updates, slow subscribers, `WaitAsync` without cache acknowledgment, disposal/unsubscription, queued but unapplied notifications, failure, cancellation, and retry. Test the distinct completion and acknowledgment semantics without assuming synchronous delivery.
5. For deliberate semantic changes, update implementation, tests, target `AGENTS.md`, public documentation, and migration guidance together.

Identify affected Usage samples/integration consumers without including their implementation procedures. Usage is a separate repository/plugin boundary; use `tplqueue-usage` when available for explicitly authorized Usage work. Identify required Core/Abstractions coordination without expanding Adapter-only edits into those repositories.

## Report

State CacheQ ownership, Observer path, `WaitAsync` behavior, cache acknowledgment behavior, disposal behavior, concurrency/race implications, affected consumers, tests/validation, and compatibility impact.
