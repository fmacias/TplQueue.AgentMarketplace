---
name: understand-tplqueue-architecture
description: Explain TplQueue package and repository boundaries, CoreApi versus Adapter IApi, construction versus execution responsibilities, and the separate Core licensing boundary. Use for consumer architecture and adoption questions, not private Core implementation or repository modification workflows.
---

# Understand TplQueue architecture

Read applicable repository instructions and current public contracts, package READMEs, Adapter public documentation, and relevant Usage examples. Prefer current repository evidence over an older skill or README; identify contradictions before dependent edits. With packages only, use their public API/documentation and state version uncertainty. Public integration must not require a private Core checkout. Never copy private Core source, tests, internal names, algorithms, synchronization, scheduler details, or private documentation into consumer guidance.

## Explain the ownership model

TplQueue provides dependency-aware Job execution, bounded-concurrency and FIFO queues, retry-aware execution, lifecycle observation, and payload-aware DataJobs with optional cache-backed integration.

| Repository | Responsibility | Consumer surface |
| --- | --- | --- |
| `TplQueue.Abstractions` | Public contract layer | `Fmacias.TplQueue.Contracts` interfaces and shared models |
| `TplQueue.Core` | Licensed execution kernel | `Fmacias.TplQueue.Core.CoreApi.Create()` and `ICoreApi` |
| `TplQueue.Adapter` | Public integration modules and integration facade | `Fmacias.TplQueue.API.Create(...)`, `IApi`, optional integration modules |
| `TplQueue.Usage` | Public runnable consumer examples and package validation | Samples and package-consumer checks, not a required runtime package |

```text
Application
  +-- CoreApi / ICoreApi      execution construction surface
  |     +-- IQFactory
  |     +-- IJobFactory
  |     +-- IDataJobFactory
  +-- Adapter API / IApi      integration facade composed around ICoreApi
```

CoreApi is a factory facade and an encapsulated composition boundary. It constructs runtime objects through public factories; those objects own execution. Repeated `CoreApi.Create()` calls do not establish singleton identity.

Core can be consumed directly when the application supplies its own composition. Adapter is the normal higher-level surface for queue configuration, retry-policy configuration and concrete factories, observer factories, serializer factories, payload-handler registration, and cache construction. `API.Create(core, retryPolicyOptions, queueOptions)` receives the Core API explicitly. Adapter does not replace Core execution or grant Core source access.

Use the Adapter pattern for adapting named queue/options configuration to the Core factory contract, the Observer pattern for `IObservable<IJobEvent>`, and Retry Policy Strategy for interchangeable `IRetryPolicy` behavior. Do not invent an application-wide architectural pattern from these local relationships.

## Keep licensing distinct

Adapter and the public AgentMarketplace plugins remain independently public/MIT as applicable. This plugin's MIT license covers the agent instructions only and does not change `TplQueue.Core` licensing. Core execution requires the applicable Core package/license terms. Private source access requires authorization and the terms established by the final Core licensing model.

The final Core license type is not finalized in this integration baseline. Consult the actual package license and maintained public licensing documentation for established conditions; state unresolved terms explicitly. Do not infer pricing, tiers, redistribution or modification rights, SLAs, or source-access terms from package availability or the plugin's MIT license.

## Navigate to the owner

| Question | Owning plugin |
| --- | --- |
| Application integration and navigation across packages | `tplqueue-overview` |
| Public Core runtime semantics | `tplqueue-core-public` |
| Adapter implementation/change workflows | `tplqueue-adapter` |
| Public contract ownership/change workflows | `tplqueue-abstractions` |
| Samples and package-consumer validation | `tplqueue-usage` |
| Generic engineering workflows | `engineering` |

Start package selection with [choose-tplqueue-packages](../choose-tplqueue-packages/SKILL.md) and composition with [integrate-tplqueue-application](../integrate-tplqueue-application/SKILL.md). For deeper public construction semantics, route to `tplqueue-core-public/use-core-factory-facade` when available. Routing does not authorize repository edits or require another plugin for basic guidance.

Adapter's `docs/<lang>/` trees remain the public product-documentation and publishing source; Usage owns sample documentation. This plugin adds integration/navigation guidance without moving those boundaries.

## Answer

Identify the owning package/repository, public entry point, application integration responsibility, runtime responsibility, and related plugin for deeper questions. Separate verified current behavior from version or license uncertainty.
