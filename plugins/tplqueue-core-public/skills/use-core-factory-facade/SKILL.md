---
name: use-core-factory-facade
description: Guide TplQueue.Core consumers through CoreApi.Create, ICoreApi, and public queue, Job, and DataJob factories, including construction boundaries and lifetime assumptions. Use for Core construction API usage; Adapter DI configuration belongs to tplqueue-adapter.
---

# Use Core factory facade

Read applicable repository instructions, current public factory contracts, and maintained public usage documentation. Follow the target version's contracts and observable behavior; surface conflicts with this baseline. Private source is optional verification evidence only, never material to copy into public guidance.

## Select the public factory

The current entry point is `Fmacias.TplQueue.Core.CoreApi.Create()`, returning `ICoreApi`. Describe it as a factory facade, controlled creation boundary, and encapsulated composition boundary.

| Factory property | Public factory | Public results |
| --- | --- | --- |
| `QFactory` | `IQFactory` | `IParallelQ`, `IFifoQ`, `ICacheQ` |
| `JobFactory` | `IJobFactory` | `IJob`, `IJobRoot` |
| `DataJobFactory` | `IDataJobFactory` | `IDataJob`, `IDataJobRoot`, and supported generic forms |

Choose the overload supported by the target contract and supply its required arguments. Consumers depend on the returned public abstractions. The facade creates/configures runtime objects; it does not execute queued work. See [queue lifecycle](../use-core-queue-lifecycle/SKILL.md) for execution and disposal, and [DataJob/Payload](../use-core-datajob-payload/SKILL.md) for payload-aware construction.

## Preserve construction semantics

- Treat static `Create` methods as controlled factories. They do not establish singleton identity; do not assume repeated calls return the same object.
- Aggregating several factory capabilities is intentional when it hides concrete internal construction types. Assess construction responsibility separately from runtime responsibility.
- Do not recommend public implementation classes, constructors, additional interfaces, or DI registration solely to eliminate construction aggregation. Do not classify factory properties alone as a service locator.
- Determine object ownership, sharing, and disposal from public contracts and application composition. Do not infer lifetime guarantees from `Create` or invent ownership of supplied dependencies.
- Keep internal factory implementations, private source, and implementation diagrams out of public guidance.

Route Adapter-provided DI integration to `tplqueue-adapter` when available, and contract changes to `tplqueue-abstractions`. Identify their impact without expanding the user's repository scope or requiring those plugins for ordinary public API guidance.

## Report

State the public entry point, requested factory, resulting public abstraction, contract-supported ownership/lifetime considerations, and relevant cross-plugin impact.
