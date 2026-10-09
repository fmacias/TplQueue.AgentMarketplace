---
name: choose-tplqueue-packages
description: Select TplQueue NuGet packages for a concrete consumer workload, including direct Core use, the Adapter facade, Microsoft DI, retries, observers, serializers, and cache providers. Use for package selection and dependency composition, not package implementation changes.
---

# Choose TplQueue packages

Identify the application's target framework, hosting/container choice, execution needs, and whether payload rehydration is needed. Read current public package READMEs and project/package dependency metadata; use relevant Usage examples as consumer evidence. Follow current evidence over stale skills or installation examples. Do not require private Core source. Do not hardcode versions unless requested and supported by evidence for the selected release.

## Select by responsibility

| Package | Role and selection criterion |
| --- | --- |
| `Fmacias.TplQueue.Abstractions` | Public contracts; sufficient for a contracts-only consumer library |
| `Fmacias.TplQueue.Core` | Execution kernel; needed by the application's runtime composition |
| `Fmacias.TplQueue` | Top-level Adapter facade for normal integrated composition |
| `Fmacias.TplQueue.Microsoft.DependencyInjection` | `IServiceCollection` integration when the host uses Microsoft DI |
| `Fmacias.TplQueue.RetryPolicies` | Built-in retry policies/factories |
| `Fmacias.TplQueue.Observers` | Built-in observer implementations exposed through factories |
| `Fmacias.TplQueue.Cache.Abstract` | Cache abstraction/support for payload persistence and rehydration |
| `Fmacias.TplQueue.Cache.MemCache` | One in-process cache provider; useful for lightweight local workloads/testing |
| `Fmacias.TplQueue.Serialization.SystemTextJson` | JSON serialization |
| `Fmacias.TplQueue.Serialization.Xml` | XML serialization |

Queue execution starts with Core. Add the facade for integrated application composition, DI for `IServiceCollection`, and the selected retry/observer modules when their implementations are needed. Cache-backed DataJob persistence/rehydration needs a compatible cache provider/support and serializer. Immediate DataJob execution alone does not require CacheQ, MemCache, or a serializer. In-process caching does not establish restart durability.

Not every application needs every module. A narrower package can be sufficient; do not add the facade solely to use one retry factory or a contracts-only library. Distinguish capabilities from explicit package references: the current facade dependency graph already includes retry, observer, serializer and MemCache modules, and the DI package depends on the facade. Neither supplies the Core execution package as a substitute for explicit application composition. Verify the target release's transitive graph before recommending redundant direct references. A transitive MemCache dependency does not mean the application must use it or that its factory is registered in DI.

## Connect retries to execution

Built-in strategies include `NoRetryPolicy`, `LinearBackoff`, and `ExponentialBackoff`. Custom `IRetryPolicy` implementations are possible through the applicable factory contract; the default abstract factory's custom-type path expects a concrete type with a public parameterless constructor. Do not promise arbitrary policy auto-discovery.

```text
named retry options + queue option RetryPolicy name
  -> Adapter IQFactoryAdapter / IRetryPolicyAbstractFactory
  -> policy creation supplied to the Core queue
  -> Core retry execution
```

Queue configuration may omit the retry name: null, empty, or whitespace selects NoRetry. Current non-generic named lookup also falls back to NoRetry for an unknown name; typed lookup has different failure behavior. Do not generalize the fallback to every factory overload. Route detailed root/queue execution precedence to `tplqueue-core-public/configure-core-retry-behavior`, and Adapter policy/configuration implementation changes to `tplqueue-adapter/modify-adapter-retry-policy` when available.

## Answer

Give the smallest useful package set, the reason for each choice, direct versus transitive dependencies, and any host/package-version checks still needed. Route composition to [integrate-tplqueue-application](../integrate-tplqueue-application/SKILL.md), legacy compatibility to [integrate-tplqueue-legacy-application](../integrate-tplqueue-legacy-application/SKILL.md), and Core's separate licensing boundary to [understand-tplqueue-architecture](../understand-tplqueue-architecture/SKILL.md).
