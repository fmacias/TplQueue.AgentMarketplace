---
name: integrate-tplqueue-web-application
description: Integrate TplQueue with ASP.NET Core, hosted services, HTTP endpoints, and SignalR using application-owned queues and observer-to-DTO transport projection. Use for web hosting and notification composition, not detailed Core observation semantics or sample-specific architecture changes.
---

# Integrate TplQueue into a web application

Read the host's instructions, current DI/public observer contracts and Adapter documentation, and relevant public Usage samples. Follow current code over stale README or skill baselines and surface differences. No private Core implementation knowledge is needed.

## Compose the host

```csharp
using Fmacias.TplQueue.Core;
using Fmacias.TplQueue.Microsoft.DependencyInjection;

builder.Services.AddTplQueue(builder.Configuration, CoreApi.Create());
```

Application services then create and own runtime queue instances. Use [DI integration](../integrate-tplqueue-dependency-injection/SKILL.md) for exact registrations and snapshots. Do not create a queue per HTTP request when callers should share execution. Inject a submission service or narrower factory/queue contract into consumers; keep construction and lifecycle in the owning host/application service.

TplQueue is independent of SignalR. Controllers, Minimal APIs, hosted services, `BackgroundService`, application services, metrics/reporting components, and persistence/projection services can submit or observe work without it. HTTP endpoints can submit Jobs to the application-owned queue; use application response DTOs rather than exposing queue/runtime objects as transport objects. Ensure dependencies retained by queued work remain valid beyond the request that submitted it.

## Project observation into SignalR

```text
TplQueue Job execution
  -> IJobEvent
  -> application IObserver<IJobEvent>
  -> transport projection DTO
  -> application notifier
  -> IHubContext<THub>
  -> SignalR clients
```

SignalR is a transport projection of TplQueue observation, separate from execution. Do not normally put broadcasting into Job bodies merely to monitor execution. Subscribe the observer to the same queue instance that receives submissions, and let the runtime owner manage the subscription.

Prefer application-owned transport DTOs containing the event information needed by clients. Do not send mutable live Jobs, queue internals, private runtime objects, or live payload references directly to browsers. When payload display is needed, explicitly produce a detached application projection rather than treating every in-process object as transport data.

The public `TplQueue.Usage/samples/QueueObserverSignalRDashboard` example demonstrates `IJobEvent -> QueueEventProjector -> QueueEventDto -> DashboardNotifier -> IHubContext`. Its observer projects then hands off asynchronous notification; the notifier handles broadcast errors. Use the separation of responsibilities without requiring those sample class names in another application. This is a distinct transport sample; do not attribute its custom hub and endpoints to the separate Blazor JobMonitor sample.

## Keep callbacks and delivery ownership explicit

Observer callbacks should remain lightweight and return quickly. Project the needed event information and hand off asynchronous work; do not use `async void` as the generic solution. Observe asynchronous failures in the application notifier or processing owner.

For workloads whose volume, ordering, or reliability needs exceed a simple sample handoff, suggest an application-owned asynchronous buffer/channel or processing service. Choose capacity, overload behavior, ordering, and shutdown handling according to the application's requirements. Do not present the sample's unawaited notification task as a guaranteed delivery, ordering, or drain mechanism, or mandate a particular buffer when unnecessary.

Route detailed observer delivery and lifecycle semantics to `tplqueue-core-public/observe-core-job-lifecycle` when available. `WaitAsync` waits queue-owned work, not observer or transport delivery, and does not guarantee CacheQ acknowledgment.

## Keep lifetime in the host

Queues and subscriptions belong to the host/application service layer. Browser connections must not implicitly create, destroy, or duplicate shared workloads unless the application explicitly chooses that behavior. Coordinate host shutdown with the queue owner and any application-owned transport handoff; browser lifetime is not queue lifetime.

## Answer

Show registration, queue/runtime ownership, the submission path, and the observer/projection/notifier path when SignalR is needed. State asynchronous delivery assumptions and route sample-specific architecture or package-consumer validation to `tplqueue-usage` rather than duplicating its workflows.
