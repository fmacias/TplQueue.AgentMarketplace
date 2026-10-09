---
name: modify-blazor-monitor-projection
description: Modify TplQueue.Usage C# Blazor Observer projections, immutable monitor snapshots, DTO mappings, and Razor/JavaScript interop while preserving host workload ownership and event-driven updates.
---

# Modify Blazor monitor projection

Complement engineering's `implement-feature`, `review-concurrency`, `testing-dotnet`, and `validate-dotnet-change`, and [review-usage-sample-architecture](../review-usage-sample-architecture/SKILL.md), when available and relevant. Keep this workflow at the backend-to-frontend projection boundary.

## Establish the current contract

Read applicable parent instructions, the target root `AGENTS.md` or `Agents.md` and delegated/scoped instructions, and its maintained Blazor frontend architecture and contract ownership guide (currently `docs/architecture/blazor-consumer-sample.md`). Inspect Observer integration, projection store, DTOs, Razor/JS bridge, runtime ownership, and relevant tests.

The target guide, source, and tests remain authoritative; surface contradictions before dependent edits. Preserve the following contracts unless the explicit task deliberately changes them.

## Preserve host and snapshot behavior

The current dashboard is a .NET 8 Interactive Server application observing host-owned workloads. Browser connections neither own nor start workloads. Stop arrivals stops future arrivals; it does not cancel accepted jobs.

Keep runtime behavior in C#. Preserve immutable snapshots, Observer isolation, `InvokeAsync` dispatch into the Blazor context, coalesced updates, asynchronous subscription disposal, and asynchronous JS interop disposal.

Load one initial snapshot, then refresh frontend data after new accepted Observer events. Do not replace this with polling or periodic live redraw requests. Advance live time on snapshot arrival while preserving paused/history references under the maintained UI model. Local user interaction and resize may redraw without a new backend snapshot.

A parent rerender with the exact same snapshot must not resend that snapshot through JS interop merely because Razor rerendered. Preserve snapshot identity handling and replace delivered instances rather than mutating them.

## Preserve metadata semantics

- `IJobExecutionEvent` is optional additive Observer metadata. Do not break existing `IJobEvent` implementations or fabricate channels for events without channel metadata. Null remains unassigned under the current presentation contract.
- Consume graph membership through the current `ISimulationGraphCatalog` contract and preserve all `rootJobIds` for shared jobs.
- Membership is not evidence of queue acceptance, queue ownership, lifecycle state, or execution channel. Keep dependency relationships distinct from membership; do not synthesize execution facts from membership.

## Review the DTO boundary

Handwritten DTOs and mappers require semantic review when meaning changes; mechanical field mapping alone is insufficient. Keep snapshots as presentation contracts rather than exposing live runtime objects.

If generation is introduced, require authoritative source inputs, pinned tooling, repeatable generation, and repeatable validation. Do not mix generated transport contracts with handwritten frontend-domain semantics without an explicit architecture decision.

Validate affected projection, snapshot, mapping, and interop behavior. Use [validate-job-monitor-interaction](../validate-job-monitor-interaction/SKILL.md) when browser/circuit behavior is involved. Identify required changes in other repositories without expanding Usage-only scope.

## Report

State the projection contract changed, snapshot behavior, Observer behavior, graph metadata impact, execution-channel impact, JS interop impact, DTO/mapping impact, and tests and validation.
