---
name: modify-adapter-public-contracts
description: Modify or review TplQueue.Adapter public contracts and coordinated API migrations, including payloads, queue factories, WaitAsync, IPayload.HandlerKey, Then extensions, and configuration APIs. Assess compatibility and affected consumers.
---

# Modify Adapter public contracts

Apply to public interfaces, classes, records, structs, methods, properties, extension methods, payload-facing contracts, queue construction/factories, and configuration contracts. Complement `review-public-api`, `implement-feature`, `refactor-code`, `testing-dotnet`, and `validate-dotnet-change` from engineering when available and relevant.

## Establish contract truth

1. Read applicable parent instructions, target root `AGENTS.md`, nearest affected package-level `AGENTS.md`, public source, tests, coordinated API migration documentation, and affected public documentation.
2. Classify the request as a compatible extension, behavioral bug fix, API migration, or deliberate breaking change. For review-only requests, report required edits without implementing them.
3. Resolve exact current behavior from the target checkout. The contract baseline includes `WaitAsync`, `IPayload.HandlerKey`, factory-based CacheQ creation, and the current moved `Then` extensions; this list does not define their signatures or behavior.

## Bound the change

- Preserve production `netstandard2.0` compatibility unless the explicit task deliberately changes the framework policy.
- Preserve namespaces unless a change is strictly necessary. Preserve public signatures unless the requested feature, bug fix, or deliberate migration requires a change.
- Preserve Job/Queue terminology. Do not introduce legacy TaskRunner names or substitute task runner/work item terminology for current Job concepts.
- Do not expose internal implementation types to solve a local implementation problem.
- When deliberately changing a contract, update implementation and tests together. Update target `AGENTS.md` when an invariant changes, applicable API migration guidance, and affected public documentation. Identify affected Adapter packages and consumers.

## Coordinate impact within scope

Core and Abstractions are separate git boundaries; explicitly identify required coordination. Usage consumes Adapter through samples and integration scenarios: identify consumer behavior or samples that may need migration without embedding Usage implementation details here. Route Usage work to `tplqueue-usage` when available.

Do not modify related repositories unless the explicit task includes them. Do not silently reconcile contradictions between repositories. Use the engineering `align-cross-repository-documentation` workflow when documentation alignment is necessary and available; report unresolved contract contradictions and required coordination within authorized scope.

## Report

State the affected contract, compatibility classification, previous and resulting behavior, affected Adapter packages and consumers, Core/Abstractions impact, Usage impact, migration impact, documentation impact, and validation performed or still required. The target repository's instructions, source, tests, and migration documentation remain authoritative.
