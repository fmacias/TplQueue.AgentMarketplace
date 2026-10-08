---
name: modify-adapter-di-registration
description: Modify TplQueue.Adapter DI settings, configuration builders, registration APIs, service factories, or Upsert behavior in Fmacias.TplQueue.Microsoft.DependencyInjection while preserving independent registration snapshots.
---

# Modify Adapter DI registration

Complement engineering's `review-csharp`, `review-public-api`, `implement-feature`, `testing-dotnet`, and `validate-dotnet-change` when available and relevant.

## Establish configuration semantics

Read applicable parent instructions, target root `AGENTS.md`, nearest DI package `AGENTS.md`, affected source/tests, and public configuration documentation. The target checkout is authoritative; surface conflicts before dependent changes.

Preserve the current model unless the explicit task deliberately changes it:

- DI settings are configuration-time builders.
- Registration captures independent option snapshots.
- Later `Upsert` calls do not reconfigure an API/service captured by an earlier registration.

## Bound mutability and ownership

1. Inspect what each registration captures. Verify that later builder mutation cannot retroactively change an earlier registration and that independent registrations do not unintentionally share mutable option state.
2. Where affected, trace singleton, scoped, and transient lifetimes, factory-created resources, and `IDisposable` ownership/disposal.
3. Preserve Adapter package boundaries and production `netstandard2.0` compatibility unless the explicit task deliberately changes the framework policy. Preserve public APIs unless the explicit task requires a deliberate contract migration.
4. Add/update applicable tests for captured snapshot independence, later `Upsert` behavior, multiple-registration isolation, factory behavior, and service lifetime/ownership.
5. If configuration semantics deliberately change, update implementation, tests, target `AGENTS.md`, public configuration documentation, and applicable migration guidance together.

## Identify composition impact

Identify changes to how Usage consumers compose Adapter packages without encoding Usage-specific composition procedures. Use the separate `tplqueue-usage` plugin when available for explicitly authorized Usage work. Identify any required Core/Abstractions coordination; do not modify another repository during Adapter-only work.

## Report

State builder semantics, captured snapshot behavior, registration isolation, service lifetime/ownership impact, public/configuration compatibility, consumer impact, and tests/validation performed or required.
