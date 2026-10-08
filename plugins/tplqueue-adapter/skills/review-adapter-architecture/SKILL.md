---
name: review-adapter-architecture
description: Review TplQueue.Adapter package ownership, facade responsibilities, dependency direction, terminology, and cross-repository architectural impact. Use for Adapter architecture reviews or changes crossing its package boundaries.
---

# Review Adapter architecture

Review and report within TplQueue.Adapter. Complement the engineering plugin's `architecture-review`, `review-csharp`, `review-public-api`, and `review-concurrency` when available and relevant; keep this review focused on Adapter-specific boundaries.

## Establish the current architecture

1. Read applicable parent `AGENTS.md` before the target repository root `AGENTS.md`, then the nearest package-level instructions for every affected Adapter package.
2. Inspect the actual solution, projects, references, source, tests, and relevant architecture documentation. Discover the current package structure; do not infer a fixed package inventory from this skill.
3. Treat the target checkout's instructions, source, tests, and applicable documentation as authoritative. Surface conflicting evidence rather than silently choosing a new architectural model.

## Assess Adapter boundaries

- Treat every production package as a first-class boundary. Keep `Fmacias.TplQueue` a thin facade; do not move specialized adapter implementation into it for convenience.
- Check circular dependencies, incorrect dependency direction, responsibility leakage, duplicated implementation, exposed implementation types, convenience references that weaken ownership, facade implementation growth, and coupling between unrelated Adapter packages.
- Preserve production `netstandard2.0` compatibility unless the explicit task deliberately changes the framework policy.
- Preserve official terminology: `IJob`, `IJobRoot`, `IDataJob`, `IDataJobRoot`, `IParallelQ`, `IFifoQ`, and `ICacheQ`. Treat `TaskRunner`, `TaskRunnerRoot`, and related names as legacy; do not rename current Job concepts to task runners or work items.
- Preserve controlled asynchronous/concurrent execution, strict FIFO dispatch where required, parallel dispatch where allowed, retry-policy-driven execution, and Observer-based visibility. Preserve optional data payloads and cache-backed persistence before memory-based dispatch. Keep monitoring/front-end integration from coupling UI concerns into queue execution.

## Respect repository and extension boundaries

TplQueue.Adapter, TplQueue.Core, TplQueue.Abstractions, and TplQueue.Usage are separate git boundaries. Explicitly identify any Core/Abstractions change required to resolve an Adapter issue; do not assume or change another repository's behavior within Adapter-only work.

Identify Usage sample/integration impact as consumer impact. Modify Usage only when the explicit task spans both repositories, and use `tplqueue-usage` for Usage-specific work when available. Keep Usage implementation, UI architecture, project structure, and execution procedures outside Adapter skills.

Adapter subcomponents remain in `tplqueue-adapter`. Do not automatically create a skill per package. Additional skills require a distinct reusable workflow or behavioral model established by package-level `AGENTS.md` that is not adequately covered by engineering skills, Adapter root instructions, or the existing seven Adapter skills. Keep justified additions here unless the capability becomes independently useful enough to warrant a separate plugin.

## Report

For each finding, state the architectural issue, affected packages, violated boundary/responsibility and supporting evidence, public API impact, dependency impact, related repository impact, and safest bounded recommendation. Do not recommend broad restructuring merely because another architecture is possible.
