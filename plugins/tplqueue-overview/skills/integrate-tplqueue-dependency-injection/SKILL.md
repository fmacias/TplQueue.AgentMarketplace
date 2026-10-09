---
name: integrate-tplqueue-dependency-injection
description: Explain current AddTplQueue registrations and lifetimes, configuration snapshots, narrow service injection, and application-owned queue/cache/handler composition. Use for consuming Microsoft DI integration, not changing its implementation.
---

# Integrate TplQueue dependency injection

Read current public contracts, the DI package README and registration source when available, and the application's composition root. The baseline below follows current `ServiceCollectionExtensions.AddTplQueue`; prefer current repository evidence over older documentation and report differences. Public consumption must not require private Core source.

## Supply the Core API

```csharp
using Fmacias.TplQueue.Core;
using Fmacias.TplQueue.Microsoft.DependencyInjection;

services.AddTplQueue(configuration, CoreApi.Create());
```

The application supplies `ICoreApi`. `AddTplQueue` neither discovers nor independently creates the licensed Core implementation. The current API also supports an overload taking only the supplied Core API, a configuration callback, and explicit dictionaries; check the target overload rather than copying an older signature.

## Distinguish registrations from runtime objects

Current shared/singleton registrations are:

- `IApi`
- `IReadOnlyDictionary<string, IRetryPolicyOptions>`
- `IReadOnlyDictionary<string, IQOptions>`
- `IRetryPolicyAbstractFactory`
- `IJobFactory`
- `IDataJobFactory`
- `IQFactoryAdapter`
- `IObserverFactory`
- `ISystemTextJsonSerializerFactory`
- `IXmlSerializerFactory`
- `ITplQueueSettings`

Current transient registrations are `ISystemTextJsonUniversalSerializer` and `IXmlUniversalSerializer`. Do not infer a registration for a base interface solely because a registered interface inherits it.

The integration does not automatically register `ICoreApi`, concrete queues, concrete CacheQ instances, application-specific cache factories or caches, arbitrary application observers, or application payload handlers. Factory/options registration is not queue-instance registration. Those objects remain explicit application composition responsibilities unless another integration module deliberately owns them. Do not invent auto-discovery or additional registrations.

Inject `IJobFactory`, `IDataJobFactory`, `IQFactoryAdapter`, or `IObserverFactory` where sufficient. Reserve `IApi` for consumers that need several facade features. See [application composition](../integrate-tplqueue-application/SKILL.md) for workload objects and ownership.

## Treat settings as configuration-time input

`ITplQueueSettings` is a configuration-time builder. `AddTplQueue` captures independent queue/retry option snapshots; the read-only dictionaries registered in DI are the facade's captured options. Later `Upsert(...)` calls do not dynamically reconfigure the registered `IApi`, captured factories/options, or existing queue instances. Do not describe this as live configuration or `IOptionsMonitor` semantics.

Configuration uses `TplQueue:RetryPolicies` and `TplQueue:Queues`. Named queue options connect a retry-policy name to Adapter policy creation and then Core execution. For selection and current fallback distinctions, see [package/retry composition](../choose-tplqueue-packages/SKILL.md); deeper execution precedence belongs to `tplqueue-core-public/configure-core-retry-behavior`.

## Register a chosen cache factory explicitly

An application choosing MemCache may add:

```csharp
using Fmacias.TplQueue.Cache.MemCache;
using Fmacias.TplQueue.Contracts;
using Microsoft.Extensions.DependencyInjection;

services.AddSingleton<IMemCacheFactory>(MemCacheFactory.Create());
```

Its cache-owning service can receive `IApi`, `IMemCacheFactory`, and the selected concrete serializer interface, then construct:

```csharp
IMemCache cache = api.Cache<IMemCache>(cacheFactory, serializer);
```

`serializer` must implement `IUniversalDataSerializer`. `IApi.Cache<T>` receives `ICacheFactory<T>` and a serializer, with an optional explicit `ITypeResolver` overload. Register required handlers on the same facade used to construct the cache. `AddTplQueue` does not register `IMemCacheFactory`; MemCache is one provider, not a requirement. See [DataJob/cache composition](../compose-datajobs-cache-and-handlers/SKILL.md).

Choose concrete queue/cache ownership from the workload and dependencies, not by extrapolating factory lifetimes. A shared queue should be created once by its runtime owner and used for both submission and observation; disposing that runtime and its resources remains explicit composition work.

## Answer

Separate existing registrations, application additions, configuration capture, and concrete runtime ownership. Identify the narrow injectable dependency and any target-version uncertainty. Route registration implementation changes to `tplqueue-adapter/modify-adapter-di-registration` when available.
