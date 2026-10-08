---
name: modify-adapter-retry-policy
description: Modify TplQueue.Adapter retry-policy configuration, factory resolution, queue policy behavior, or root-policy inheritance. Use when changes may affect optional policy names, explicit NoRetry, or inherited queue policies.
---

# Modify Adapter retry policy

Complement engineering's `review-concurrency`, `implement-feature`, `testing-dotnet`, and `validate-dotnet-change` when available and relevant.

## Establish retry behavior

Read applicable parent instructions, target root `AGENTS.md`, nearest RetryPolicies/affected package `AGENTS.md`, retry-policy source, tests, and configuration/public documentation. The target repository is authoritative; surface conflicts with this baseline before dependent changes.

Preserve current semantics unless the explicit task deliberately changes them:

| Configuration | Behavior |
| --- | --- |
| Queue policy name is null, empty, or whitespace-only | Select `NoRetry`. |
| Explicit root `NoRetry` | Override the queue retry policy. |
| Unspecified root retry policy | Inherit the queue retry policy. |
| Named retry policy | Resolve according to the current factory contract. |

Keep unspecified root policy, explicit root `NoRetry`, and named policy distinct. Do not normalize unspecified policy to explicit `NoRetry` if that changes inheritance.

## Change and validate

1. Verify factory/resolution behavior whenever names or resolution logic change.
2. Inspect affected cancellation, asynchronous dispatch, retry execution, ordering, and shared-state interactions. Preserve Adapter package ownership and production `netstandard2.0` compatibility unless the explicit task deliberately changes the framework policy.
3. Add/update applicable tests covering null, empty, whitespace-only, named, and queue-level policies; explicit root `NoRetry`; unspecified root policy and queue inheritance; invalid/unknown policies where supported; and cancellation/retry behavior when affected.
4. For deliberate semantic changes, update implementation, tests, target `AGENTS.md`, public configuration documentation, and applicable migration documentation.

Identify Core, Abstractions, or Usage API/behavior impact explicitly. These remain separate repositories; change them only within an explicitly cross-repository task. Usage-specific implementation procedures belong to the separate `tplqueue-usage` plugin when available.

## Report

State semantics preserved or changed, factory/resolution impact, root/queue inheritance behavior, concurrency/cancellation impact, public/configuration impact, affected consumers and related repositories, and tests/validation performed or required.
