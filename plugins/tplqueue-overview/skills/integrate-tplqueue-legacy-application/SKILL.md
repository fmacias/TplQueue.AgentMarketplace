---
name: integrate-tplqueue-legacy-application
description: Adopt TplQueue incrementally in .NET Framework or legacy .NET applications, validate the package graph, retain an existing composition root or IoC container, and marshal observer updates to WinForms/WPF UI threads. Use for legacy integration without requiring migration to modern .NET.
---

# Integrate TplQueue into a legacy application

Read the application's target framework, package references, composition/lifetime conventions, and applicable instructions. Verify current TplQueue public contracts, package metadata and Adapter/Usage documentation against the actual package line. Prefer current repository evidence over an older skill or README; state missing evidence. Public adoption does not require private Core source access.

## Verify compatibility for the chosen modules

TplQueue production libraries currently target `netstandard2.0`. Treat .NET Framework 4.6.1 as the technical .NET Standard 2.0 compatibility floor only. Prefer 4.7.2 or later as the practical integration baseline; Microsoft documents issues with the earlier compatibility mapping and recommends 4.7.2 or higher. See the [.NET Standard compatibility guidance](https://learn.microsoft.com/en-us/dotnet/standard/net-standard#net-standard-20).

For an existing application where upgrading is reasonable, prefer 4.8 / 4.8.1 as permitted by its deployment environment. This does not require migration to modern .NET. A `netstandard2.0` target is not evidence that every transitive dependency of every optional integration package suits every old host.

Check the selected release's actual NuGet graph, target assets, host/runtime constraints and any binding requirements. Restore/build the consumer and exercise its chosen composition before claiming compatibility. Do not infer package validation from a sibling source build. Route package-consumer checks to `tplqueue-usage/validate-usage-package-consumption` when relevant and available.

## Keep the existing composition boundary

For WinForms, WPF, Windows Service, console/service processes, or older ASP.NET without Microsoft DI:

```text
existing composition root
  -> CoreApi.Create()
  -> API.Create(core, retryPolicyOptions, queueOptions)
  -> application-owned queue runtime
  -> existing services receive public interfaces
```

Use [application composition](../integrate-tplqueue-application/SKILL.md) for supported facade signatures and narrow dependencies. Existing IoC containers or manual wiring are valid. Do not introduce Microsoft.Extensions.DependencyInjection solely because TplQueue offers an integration package.

The application owns queue instances and their subscriptions. Share a queue for a logically shared workflow rather than recreating it per UI action or request. Determine shutdown ownership from the existing host; queue waiting does not establish observer drain or CacheQ acknowledgment.

## Adopt one bounded workflow

```text
existing legacy workflow
  -> one selected TplQueue queue/workflow
  -> observe and validate
  -> expand deliberately
```

Do not require a full rewrite, conversion of every operation into a Job, replacement of the existing IoC container, immediate CacheQ adoption, SignalR, or all Adapter modules. Start from the smallest useful package set in [choose-tplqueue-packages](../choose-tplqueue-packages/SKILL.md).

Use incremental modernization or a strangler-style boundary only when those terms accurately describe the application's strategy. Replacing one subsystem incrementally does not establish that the entire application follows Strangler Fig architecture.

## Integrate UI observation

Treat observer callbacks as background/runtime notifications. For WinForms/WPF, marshal updates through the application's UI synchronization mechanism; do not assume callbacks run on the UI thread. Keep callbacks quick, capture the required data, and let the UI layer own rendering. Route deeper callback semantics to `tplqueue-core-public/observe-core-job-lifecycle` when available.

## Answer

State the target framework and package checks, selected incremental workflow, retained composition/container choice, queue owner, and UI dispatch/shutdown responsibilities. Distinguish a compatible target-framework declaration from an actually validated deployment.
