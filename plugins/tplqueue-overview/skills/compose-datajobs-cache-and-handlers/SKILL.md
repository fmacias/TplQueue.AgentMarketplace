---
name: compose-datajobs-cache-and-handlers
description: Compose IDataJob/IDataJobRoot, payload identity and HandlerKey routing, Adapter handler registration, serializers, cache factories, and CacheQ from the application boundary. Use for direct versus rehydrated DataJob integration, not private CacheQ implementation or detailed runtime semantics.
---

# Compose DataJobs, cache and handlers

Read current `IDataJobFactory`, `IPayload`, `IHandler`, `IApi`, `ICacheFactory<T>`, `IDataJobCache` and queue factory contracts, applicable Adapter package READMEs, and relevant Usage examples. Follow current public evidence over stale skill/README baselines and report contradictions. Never require or publish private Core implementation details.

## Create a DataJob with its handler

```text
IPayload + IHandler
  -> IDataJobFactory
  -> IDataJob / IDataJobRoot
```

For immediate in-process execution, supply the handler directly. With an `IPayload payload`, `IHandler handler`, and factory obtained from CoreApi, IApi, or DI, current construction includes:

```csharp
IDataJob job = dataJobFactory.DataJob(payload, handler, name: "process-input");
IDataJobRoot root = dataJobFactory.DataJobRoot(payload, handler, name: "workflow");
```

Generic forms and overloads with explicit Job IDs are available; use the target contract rather than inventing constructors. A direct DataJob does not need cache storage, handler auto-discovery, or a serializer just to execute.

| Identity | Meaning |
| --- | --- |
| `PayloadId` | Payload-instance identity |
| `HandlerKey` | Stable identifier for handler-routing behavior |
| `JobId` | Job execution/lifecycle identity |

Do not automatically equate these identities. `IHandler.HandleAsync(IPayload, CancellationToken)` supplies executable behavior for the data. Payload identity and serialized input are separate from Job lifecycle.

## Register handlers for rehydration

Direct construction can receive the handler explicitly. Persistence/rehydration needs a way to recover executable behavior from the stable persisted `IPayload.HandlerKey`:

```text
dehydrated payload HandlerKey
  -> Adapter handler registry
  -> executable handler behavior on hydration
```

The current `IApi` registration overloads are:

```csharp
IApi RegisterPayloadHandler(string payloadHandlerKey, IHandler handler);
IApi RegisterPayloadHandler(string payloadHandlerKey, Func<IHandler> handlerFactory);
IApi RegisterPayloadHandler(string payloadHandlerKey,
    Func<IPayload, CancellationToken, Task> handler);
IApi RegisterPayloadHandler<TPayload>(string payloadHandlerKey,
    Func<TPayload, CancellationToken, Task> handler) where TPayload : IPayload;
```

Register handlers on the facade used to create the cache before hydration can need them. Keep keys stable for payloads that outlive a deployment; retain compatible handlers for older persisted keys when introducing an incompatible payload/handler version.

There is no automatic DI handler discovery or assembly scanning. If handlers need container services, the application must compose those dependencies and scopes explicitly. A synchronous `Func<IHandler>` factory alone does not define asynchronous execution-scope disposal; avoid capturing a request-scoped service in a long-lived handler.

## Supply a cache provider and serializer

`IApi.Cache<T>` receives an `ICacheFactory<T>` plus `IUniversalDataSerializer`, and optionally an explicit `ITypeResolver`. The facade supplies the other TplQueue dependencies required for cache construction. The resulting `T` must implement `IDataJobCache`.

```text
ICacheFactory<T> + serializer (+ explicit ITypeResolver when needed)
  -> IApi.Cache<T>(...)
  -> IDataJobCache implementation
  -> Core QFactory.CacheQ(...) composition
```

The current `CacheQ` factory accepts a `Func<IDataJobCache>`, logger, and `IParallelQ`. Keep those dependencies explicit and establish their ownership from the public contract. Do not infer cache or dispatcher disposal ownership merely from passing them into a factory.

MemCache is one in-process provider, not mandatory and not a promise of durable storage. For it, an application may register `IMemCacheFactory` explicitly and inject it alongside `IApi` and the selected serializer, then call `api.Cache<IMemCache>(cacheFactory, serializer)`. See [DI integration](../integrate-tplqueue-dependency-injection/SKILL.md). `AddTplQueue` does not automatically register application cache factories or cache instances.

## Choose the queue by workload

| Queue | Integration role |
| --- | --- |
| `ParallelQ` | Bounded-concurrency in-memory execution |
| `FifoQ` | Strict serial/FIFO execution |
| `CacheQ` | Payload-aware cache-backed execution/rehydration through `IDataJobCache` |

CacheQ is not a database. Its storage characteristics depend on the chosen cache implementation; use it when the workload needs the cache-backed path rather than merely because it uses DataJobs.

Queue `WaitAsync` waits queue-owned work; it is neither observer drain nor cache acknowledgment. Route detailed behavior to `tplqueue-core-public/use-core-cacheq-semantics`, and construction/identity/rehydration contract details to `tplqueue-core-public/use-core-datajob-payload` when available. Adapter implementation changes and contract changes belong to `tplqueue-adapter` and `tplqueue-abstractions` respectively.

## Answer

Identify immediate versus rehydrated execution, payload and Job identities, handler registration and dependency ownership, chosen serializer/provider/factory, queue owner, and any acknowledgment requirement. Keep detailed retry, observer and CacheQ runtime semantics in their owning plugin.
