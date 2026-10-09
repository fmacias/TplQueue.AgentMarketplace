---
name: review-usage-sample-architecture
description: Review TplQueue.Usage sample module ownership, dependency direction, host composition, service lifetimes, queue identity, and public package-consumer boundaries. Use for Usage sample architecture reviews or changes crossing these boundaries.
---

# Review Usage sample architecture

Review and report within TplQueue.Usage. Complement engineering's `architecture-review`, `review-csharp`, `review-public-api`, and `review-concurrency` when available and relevant; leave generic engineering guidance to those skills.

## Establish the current architecture

Read applicable parent workspace instructions, the target root `AGENTS.md` or `Agents.md`, files it delegates to, and more-specific instructions for the affected sample, tool, or module. Before reviewing or changing the Blazor host or ETL integration, read the maintained architecture guide referenced by those instructions (currently `docs/architecture/blazor-consumer-sample.md`, relative to the target repository).

Inspect actual project references, service registration, source, and relevant tests before judging architecture. The target checkout's instructions, source, tests, and maintained documentation remain authoritative; surface contradictory evidence rather than silently imposing a different model.

Treat Usage as the public package-consumption, sample, and validation surface. Assess public consumer behavior without importing private Core implementation assumptions or documentation.

## Assess module ownership

Preserve this ownership model unless the explicit task deliberately changes it:

| Module | Responsibility |
| --- | --- |
| Domain | Jobs, payloads, handlers, queue/cache wrappers, and business data. |
| ETL Contracts | Public sample interfaces. |
| Simulation | Measurements, workflows, submission, graph tracking, and shared runtime; references Contracts rather than Domain. |
| Blazor host | Composition of these modules. |

Do not move responsibilities between modules merely for convenience.

## Check lifetimes and queue identity

Domain wrappers are transient. The singleton `IEtlQueueRuntime` captures the actual queue instances used for submission. Observers and the dashboard catalog must obtain those same instances from the runtime, never fresh wrappers or alternate queues resolved for monitoring.

For any lifetime change, assess queue identity, observer attachment, runtime ownership, disposal ownership, and resulting sample behavior. Preserve existing composition methods and lifetimes unless the requested change requires otherwise.

## Respect sample and repository boundaries

The current Blazor sample is one profile. The future TypeScript/OpenAPI architecture is a separate future profile, not a mandatory intermediary. Do not force Blazor through it without an explicit task; use one chosen presentation framework per example.

`QueueObserverSignalRDashboard` is a separate existing transport sample. Do not attribute its endpoints, custom hub, transport architecture, or responsibilities to the Blazor timeline sample.

Usage, Adapter, Abstractions, and Core are independent git boundaries. If a Usage issue requires a change elsewhere, identify the dependency and owning repository explicitly. Do not silently alter another repository's contract or expand Usage-only work. Use the corresponding repository-specific plugin when available for authorized work there.

## Report

For each finding, state the architectural issue and evidence, affected Usage modules, ownership/boundary involved, service-lifetime impact, queue/runtime identity impact, package-consumer impact, related repository impact, and safest bounded recommendation. Do not recommend broad redesign merely because another architecture is possible.
