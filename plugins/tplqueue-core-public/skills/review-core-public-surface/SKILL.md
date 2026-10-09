---
name: review-core-public-surface
description: Review TplQueue.Core public package architecture, factory boundaries, and consumer-visible runtime semantics for compatibility and accidental exposure of private implementation knowledge. Use for public Core API or usage guidance reviews, not private implementation development.
---

# Review Core public surface

Complement engineering's `architecture-review`, `review-public-api`, `review-csharp`, and `review-concurrency` when available and relevant; leave generic review procedure there.

## Establish the public baseline

Read applicable repository instructions, current public contracts, maintained public documentation, and relevant consumer evidence. These remain authoritative over this skill's baseline; surface contradictions before dependent edits. Public use must not require private source access. Authorized private-source inspection may verify observable behavior, but publish only consumer-safe conclusions.

Treat Core as the execution kernel providing Job/JobRoot and DataJob/DataJobRoot execution, ParallelQ, FifoQ, CacheQ integration, queue lifecycle, retry-policy execution points, and observable Job lifecycle events.

## Assess the construction boundary

Recognize `CoreApi.Create()` returning `ICoreApi` as a factory facade over public queue, Job, and DataJob factory abstractions. It constructs configured runtime objects; it does not itself execute queued work.

Concentrated construction responsibility can be a cohesive, controlled creation and encapsulated composition boundary. Where concrete implementations are intentionally internal, aggregation of factory capabilities is not by itself an SRP or OCP defect. Factory properties alone do not make the facade a service locator.

Flag unrelated runtime behavior, mutable global state, unrelated policy, or inappropriate coupling accumulating at that boundary. Assess construction responsibility separately from runtime responsibility. Do not recommend exposing internal concrete implementations, public constructors, extra interfaces, or DI registration solely to make construction or substitution easier.

## Keep ownership explicit

| Concern | Owner / workflow |
| --- | --- |
| Public contract ownership or changes | `TplQueue.Abstractions` / `tplqueue-abstractions` |
| Concrete retry policies, logging observers, serializers, cache implementations, DI composition, public product documentation | `TplQueue.Adapter` / `tplqueue-adapter` |
| Runnable examples and package-consumer validation | `TplQueue.Usage` / `tplqueue-usage` |
| Core package usage and observable runtime semantics | `tplqueue-core-public` |
| Private execution-kernel implementation | Private Core work, outside this public plugin |

Review public guidance for private algorithms, synchronization or lock structure, internal collections or buffering, scheduler structure, private types, source/test snippets, diagrams, and maintainer-only knowledge. Exclude such material even when it was used as verification evidence. Describe only public interfaces, factories, documented guarantees, and behavior consumers can observe and rely on; do not turn implementation accidents into guarantees.

Use the focused sibling skills for [construction](../use-core-factory-facade/SKILL.md), [queue lifecycle](../use-core-queue-lifecycle/SKILL.md), [observation](../observe-core-job-lifecycle/SKILL.md), [retry](../configure-core-retry-behavior/SKILL.md), [CacheQ](../use-core-cacheq-semantics/SKILL.md), and [DataJob/Payload](../use-core-datajob-payload/SKILL.md) when that semantic is involved.

## Report

For each finding, state the public surface, consumer-visible semantic, architectural pattern or boundary, compatibility/usage risk, owning repository, whether the guidance would expose private knowledge, and safest bounded recommendation. Report private implementation findings as private Core work without reproducing private evidence in public output.
