---
name: review-abstractions-architecture
description: Review TplQueue.Abstractions as the shared public contract boundary, including abstraction ownership, implementation coupling, terminology, compatibility, and downstream architectural impact.
---

# Review Abstractions architecture

Review and report within TplQueue.Abstractions. Complement engineering's `architecture-review`, `review-public-api`, `review-csharp`, and `review-concurrency` when available and relevant; focus here on Abstractions-specific responsibilities.

## Establish the contract boundary

1. Read applicable parent workspace `AGENTS.md` before the target root `AGENTS.md`, then the nearest package-level instructions for affected areas.
2. Inspect the actual solution, project references, public source, tests, README, and applicable architecture/migration documentation. Discover current project/package structure; do not assume a fixed inventory.
3. Treat target instructions, source, tests, and applicable documentation as authoritative. Surface conflicting evidence instead of silently redefining the architecture.

## Assess abstraction responsibilities

Treat Abstractions primarily as TplQueue's reusable public contract layer. Check for:

- Concrete implementation details leaking into contracts or unnecessary dependencies on implementation packages.
- Mutable implementation state exposed publicly and responsibilities that belong in Core or Adapter.
- Duplicate or competing abstractions, terminology divergence, and avoidable downstream coupling.
- Contracts that constrain implementation evolution beyond the behavior they need to express.

Preserve the current architecture unless the explicit task deliberately changes it. Do not redesign abstractions merely because another contract shape is possible.

Production contracts intentionally target `netstandard2.0`. Preserve compatibility with older .NET Framework and modern .NET consumers within that target's limits unless the explicit task deliberately changes framework policy. Modern C# syntax enabled by repository configuration does not authorize incompatible API/runtime requirements.

Preserve official terminology: `IJob`, `IJobRoot`, `IDataJob`, `IDataJobRoot`, `IParallelQ`, `IFifoQ`, and `ICacheQ`. Treat `TaskRunner`, `TaskRunnerRoot`, and related historical names as legacy; do not introduce task runner or work item as parallel names for current Job abstractions.

Keep contracts compatible with controlled asynchronous/concurrent execution, FIFO execution where required, parallel execution where allowed, retry-capable and observable execution, optional payload-oriented jobs, and optional cache-backed dispatch integration. Support monitoring/integration without coupling UI concerns into contracts.

## Identify downstream impact

Abstractions, Core, Adapter, and Usage are independent git boundaries. Identify each affected consumer and its architectural assumptions explicitly. Do not silently change consumer assumptions or modify related repositories during Abstractions-only work. Use the corresponding repository-specific plugin when available for authorized consumer work; Usage-specific sample/integration work belongs to `tplqueue-usage`.

Keep consumer implementation details outside Abstractions skills. Do not automatically create a skill per type or folder. Additional `tplqueue-abstractions` skills require a distinct reusable contract workflow or non-trivial behavioral invariant established by repository or nested `AGENTS.md` and not adequately covered by engineering or these three skills. Simple interface definitions, naming rules, and project inventory belong in repository instructions.

## Report

For each finding, state the architectural issue, affected contracts, abstraction responsibility and supporting evidence, compatibility impact, Core impact, Adapter impact, Usage impact, and safest bounded recommendation.
