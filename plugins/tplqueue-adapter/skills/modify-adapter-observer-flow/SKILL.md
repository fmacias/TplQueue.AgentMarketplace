---
name: modify-adapter-observer-flow
description: Modify TplQueue.Adapter Observer integration and Job lifecycle notifications, including Fmacias.TplQueue.Observers, IQ.Subscribe, Enqueued publication, subscriptions, and shared observer hub behavior.
---

# Modify Adapter Observer flow

Complement engineering's `review-concurrency`, `review-public-api`, `implement-feature`, `testing-dotnet`, and `validate-dotnet-change` when available and relevant.

## Establish the notification path

Read applicable parent instructions, target root `AGENTS.md`, nearest affected package `AGENTS.md`, observer contracts and implementation, affected source/tests, and public documentation. Treat those target artifacts as authoritative; surface contradictions before changing the notification model.

Preserve the subscription-based architecture:

- Use `IQ.Subscribe` for all Job lifecycle notifications, including `Enqueued`.
- `IQ.OnJobEventChanged` has been removed; do not reintroduce it.
- Do not recreate an inline event-handler route in `Publish` or enqueue as an alternative notification mechanism. Lifecycle events follow the observer/subscription path.
- Do not make queue execution or completion implicitly wait for Observer delivery unless the explicit task deliberately changes that contract.

## Inspect lifecycle and concurrency

Trace affected asynchronous publication, job-event propagation, actual ordering guarantees, subscription lifetime, unsubscription, disposal, concurrent publication, slow subscribers, subscriber exceptions, and shared observer hub processing. Claim only ordering guarantees supported by contracts, implementation, and tests.

Preserve Adapter package boundaries and production `netstandard2.0` compatibility unless the explicit task deliberately changes the framework policy. For a deliberate public or observable behavior change, update affected contracts, tests, public documentation, and applicable invariant/migration guidance together.

If CacheQ cache transitions or its private subscription are affected, also apply [modify-adapter-cacheq-lifecycle](../modify-adapter-cacheq-lifecycle/SKILL.md). Account for its private observer ownership, asynchronous cache updates, and lack of a cache-acknowledgment barrier in `WaitAsync` or `Dispose`.

## Validate and identify consumers

Add/update tests proving the intended subscription notification path and affected lifecycle/concurrency behavior. Do not validate by restoring the removed inline event path.

Identify potential Usage monitoring, dashboard, sample, or integration impact when public contracts or observable behavior change. Keep Usage-specific implementation out of this workflow; use `tplqueue-usage` when available for an explicitly authorized Usage task. Identify any Core/Abstractions coordination without silently modifying those separate repositories.

## Report

State the notification path, subscription behavior, asynchronous behavior, ordering guarantees actually preserved, compatibility implications of the removed path, CacheQ impact, Usage/consumer impact, tests and validation, and public API impact.
