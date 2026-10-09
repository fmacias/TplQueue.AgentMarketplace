---
name: integrate-tplqueue-application
description: Compose TplQueue in an application with or without Microsoft DI, choose narrow injected interfaces, and assign queue/runtime ownership. Use for host-independent integration and shared queue lifetime questions, not detailed Core lifecycle or Adapter modification procedures.
---

# Integrate TplQueue into an application

Read applicable application instructions, current `ICoreApi`, `IApi`, and factory contracts, the Adapter facade package README, and relevant public Usage composition examples. Current repository evidence overrides older skills or documentation. Use public surfaces only; a private Core checkout is not a prerequisite.

## Compose at the application boundary

For direct composition without Microsoft DI:

```text
composition root
  -> CoreApi.Create()
  -> API.Create(core, retryPolicyOptions, queueOptions)
  -> configure/create queues, retries, observers, serializers and caches as needed
  -> pass narrow public interfaces into application services
```

The current facade accepts read-only retry/queue option dictionaries. A minimal facade construction fragment is:

```csharp
using System.Collections.Generic;
using Fmacias.TplQueue;
using Fmacias.TplQueue.Contracts;
using Fmacias.TplQueue.Core;

ICoreApi core = CoreApi.Create();
IApi api = API.Create(
    core,
    new Dictionary<string, IRetryPolicyOptions>(),
    new Dictionary<string, IQOptions>());
```

Populate dictionaries before constructing the configured runtime when named options are needed. Do not infer DI snapshot guarantees for every direct facade input. Core-only composition may use `core.QFactory`, `core.JobFactory`, and `core.DataJobFactory` directly with explicit dependencies; route detailed construction to `tplqueue-core-public/use-core-factory-facade` when available.

For an existing `IServiceCollection` host, use [integrate-tplqueue-dependency-injection](../integrate-tplqueue-dependency-injection/SKILL.md). A DI container is optional; preserve an existing composition root or IoC container when sufficient.

## Pass the narrowest useful dependency

| Consumer responsibility | Useful dependency |
| --- | --- |
| Creates Jobs | `IJobFactory` |
| Creates DataJobs | `IDataJobFactory` |
| Creates configured Adapter queues | `IQFactoryAdapter` |
| Creates queues directly from Core | `IQFactory` |
| Creates built-in observers | `IObserverFactory` |
| Uses several Adapter composition features | `IApi` |
| Submits to an already shared workflow | Application submission service or the required queue contract |

Treat `IApi` as a composition facade, not an application-wide service locator. A service that only submits work need not receive a queue factory. Treat Jobs as workload objects constructed for the workload, not general container services.

## Assign queue ownership

Factory registration does not register queue instances. The application creates and owns queues and their subscriptions. A logically shared execution queue should normally have an application/runtime lifetime matching its workload; a singleton host runtime is a common way to express that ownership, not a universal library lifetime requirement.

Use the same queue instance for submission and observation. Do not recreate a shared queue per HTTP request, UI interaction, or monitoring resolution. Reusing a configured queue name or ID does not itself establish shared object identity. Assign startup, shutdown and disposal to the runtime owner; account for the lifetime of dependencies retained by accepted work, including services whose request scope may have ended.

`WaitAsync` waits for queue-owned work. It does not drain observers and does not guarantee CacheQ acknowledgment. Application shutdown must account separately for any application-owned observation/transport handoff and persistence requirements. Route the actual lifecycle rules to `tplqueue-core-public/use-core-queue-lifecycle`; do not reproduce detailed Core retry, observer, CacheQ or lifecycle semantics here.

## Answer

Show the composition entry point, selected interfaces, queue/runtime owner, shared instance path for submission and observation, and shutdown responsibilities. Route host-specific composition to [web integration](../integrate-tplqueue-web-application/SKILL.md) or [legacy integration](../integrate-tplqueue-legacy-application/SKILL.md), and payload/cache wiring to [DataJob composition](../compose-datajobs-cache-and-handlers/SKILL.md).
