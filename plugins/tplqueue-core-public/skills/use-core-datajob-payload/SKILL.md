---
name: use-core-datajob-payload
description: Guide current TplQueue.Core IDataJob and IDataJobRoot construction through IDataJobFactory, IPayload identity and HandlerKey routing, IHandler responsibility, and supported rehydration, retry, and CacheQ relationships.
---

# Use Core DataJob/Payload

Read applicable repository instructions, current public DataJob/payload/handler/factory contracts, maintained public documentation, and relevant consumer evidence. This skill describes the current baseline, not planned refinements. Follow the target version and surface contradictions. Authorized private inspection may verify public behavior; never expose private wrappers, handler adapters, execution, or serialization implementation.

## Separate execution from input data

Use `IDataJob`, `IDataJobRoot`, `IDataJobFactory`, `IPayload`, and `IHandler`, including generic forms where supported. Do not reintroduce TaskRunner terminology.

The Job carries execution/lifecycle identity; its payload carries serializable business/input data for handler execution. Do not merge lifecycle state into the payload unless the public contract requires it. `PayloadId` is not automatically `JobId`; an application may deliberately map them, but they are separate concepts.

| `IPayload` member | Current meaning |
| --- | --- |
| `PayloadId` | Stable payload-instance identity |
| `CollectionTime` | Collection timestamp carried by the payload |
| `HandlerKey` | Stable handler-routing identifier carried by the payload |

Do not claim Core performs every `HandlerKey` lookup. Adapter or application composition may resolve the handler. The public `IHandler.HandleAsync(IPayload, CancellationToken)` executes the supplied payload's behavior with execution cancellation input; construction receives a handler explicitly.

## Use the supported factory overload

Obtain `IDataJobFactory` from the [Core factory facade](../use-core-factory-facade/SKILL.md). It constructs payload-aware Jobs and JobRoots from payload and handler, with optional explicit Job identity and optional root retry policy where the overload supports them.

The current factory also supports DataJob construction from public `IJobNodeRecord` information together with payload and handler. Treat this as contract-supported rehydration; do not extrapolate it into arbitrary graph restoration, handler lookup, or serializer support. Determine serialization format, type resolution, and handler composition from the public contracts and the selected Adapter/application integration.

DataJobRoot follows [Core retry selection](../configure-core-retry-behavior/SKILL.md). CacheQ consumption uses payload-aware Job graphs; use [CacheQ semantics](../use-core-cacheq-semantics/SKILL.md) for acknowledgment and lifecycle behavior instead of duplicating those rules.

## Maintain an evolving baseline

For a deliberately changed DataJob/Payload contract, coordinate Abstractions contract updates first where appropriate, Core behavior and relevant tests, Adapter integration, and Usage examples; then update this skill to the resulting current behavior. Identify the owning repositories without expanding the authorized edit scope. Use their corresponding plugins when available for authorized work.

Do not pre-document proposed behavior as current behavior or copy private implementation knowledge while refreshing the skill.

## Report

State Job/DataJob type, payload identity, `HandlerKey` meaning, handler responsibility, retry and cache impact, and only the rehydration/serialization assumptions supported by the current contract.
