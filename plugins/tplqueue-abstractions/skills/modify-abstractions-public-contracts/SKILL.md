---
name: modify-abstractions-public-contracts
description: Modify public contracts owned by TplQueue.Abstractions, including Job/Queue, event, payload, WaitAsync-facing, and extension-facing contracts. Assess compatibility, coordinated migrations, and Core, Adapter, and Usage consumer impact.
---

# Modify Abstractions public contracts

Apply to public interfaces such as `IJob`, `IJobRoot`, `IDataJob`, `IDataJobRoot`, `IParallelQ`, `IFifoQ`, `ICacheQ`, `IJobEvent`, and `IPayload`, and related enums, records, structs, or supporting abstractions. Complement engineering's `review-public-api`, `implement-feature`, `refactor-code`, `testing-dotnet`, `validate-dotnet-change`, and `align-cross-repository-documentation` when available and relevant.

## Establish ownership and compatibility

1. Read applicable parent instructions before target root `AGENTS.md`, then nearest package-level instructions, affected public source/tests, relevant README/documentation, and applicable coordinated API migration guidance.
2. Classify the change as a compatible extension, behavioral clarification, bug fix, coordinated migration, or deliberate breaking contract change.
3. Preserve production `netstandard2.0` compatibility unless the explicit task deliberately changes framework policy. Preserve namespaces unless changing them is strictly necessary, and minimize public API changes.
4. Preserve official Job/Queue terminology; do not introduce legacy TaskRunner names or work item/task runner alternatives for current Job abstractions.
5. Express required behavior without unnecessarily constraining Core or Adapter implementations. Do not expose concrete implementation details merely to make a consumer easier to implement.

## Resolve coordinated migration concerns

The current source line includes `WaitAsync`, `IPayload.HandlerKey`, current `Then` extension ownership/location, and CacheQ creation changes where they interact with public contracts. Determine actual ownership from target source and project structure; not every coordinated concern belongs to Abstractions.

Use current coordinated API migration documentation when these areas are affected. Do not copy Adapter/Core runtime semantics into Abstractions merely because they participate in the same migration. Target instructions, source, tests, and applicable documentation remain authoritative; do not silently reconcile contradictory contracts across repositories.

For execution-channel event metadata, also apply [modify-execution-channel-observation](../modify-execution-channel-observation/SKILL.md).

## Change and coordinate within scope

- For every changed contract, identify likely Core, Adapter, and Usage consumers, compilation/API migration impact, and behavioral assumptions consumers may need to adapt.
- Make authorized Abstractions source changes and add/update applicable Abstractions tests. Update repository `AGENTS.md` if an invariant becomes obsolete and update applicable migration information.
- Identify required public documentation changes. Adapter owns the current publishable public documentation trees; a change to documented behavior requires an explicit Adapter documentation impact assessment.
- Do not modify other repositories during an Abstractions-only task. Route authorized repository-specific changes through the corresponding plugin when available. Identify Usage sample/integration impact while leaving its implementation and documentation procedures to `tplqueue-usage`.
- Distinguish local validation from consumer validation still required in another repository. Do not claim migration or alignment is complete without evidence.

## Report

State the contract changed, compatibility classification, previous and resulting contract shape, `netstandard2.0` impact, Core impact, Adapter impact, Usage impact, migration impact, public documentation impact, and validation performed or required.
