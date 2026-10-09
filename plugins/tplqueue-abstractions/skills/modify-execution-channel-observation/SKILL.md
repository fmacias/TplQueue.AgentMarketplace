---
name: modify-execution-channel-observation
description: Modify TplQueue.Abstractions optional IJobExecutionEvent and nullable ExecutionChannel metadata, including IJobEvent compatibility, queue-local capacity observation, retry stability, and terminal-event semantics. Execution channels are diagnostic metadata, not scheduling controls.
---

# Modify execution-channel observation

Complement [modify-abstractions-public-contracts](../modify-abstractions-public-contracts/SKILL.md) and engineering's `review-public-api`, `review-concurrency`, `testing-dotnet`, and `validate-dotnet-change` when available and relevant. Keep this workflow focused on the optional Abstractions observation contract.

## Establish the current contract

Read applicable parent instructions before target root `AGENTS.md`, nearest scoped instructions, `IJobExecutionEvent` and `IJobEvent` source, affected tests, and relevant contract/migration documentation. The target repository remains authoritative; surface conflicts before dependent edits.

Preserve these semantics unless the explicit task deliberately changes them:

| Contract aspect | Required meaning |
| --- | --- |
| Optional interface | `IJobExecutionEvent` adds nullable `ExecutionChannel` metadata without changing existing `IJobEvent`. |
| Null channel | No execution channel is assigned or exposed for that event. |
| Non-null channel | Identifies queue-local execution capacity. |
| Retries | The channel remains stable while the same execution capacity remains held. |
| Terminal events | Capture the channel before that capacity is released. |
| Pre-execution events | Remain unassigned. |
| Legacy events | Remain unassigned unless they explicitly implement the optional execution-channel contract. |

`ExecutionChannel` is diagnostic metadata, never a thread ID or a scheduling-control mechanism. Do not assume a channel equals an operating-system thread, that channel numbers are globally unique, or that consumers may use channel identity to alter dispatcher scheduling or queue execution.

Preserve the optional-interface approach while it remains the repository design. Do not add `ExecutionChannel` directly to `IJobEvent` for convenience unless the explicit task deliberately changes the compatibility model. Preserve production `netstandard2.0` compatibility unless the explicit task deliberately changes framework policy.

## Validate and coordinate

When changing the contract, verify applicable cases:

- Events without `IJobExecutionEvent` and legacy event consumers remain compatible.
- Null metadata retains its meaning; assigned execution events expose the expected channel.
- Retry events preserve the channel while capacity remains held.
- Terminal events retain the channel captured before release.
- Pre-execution events remain unassigned.

Identify Core event-production impact, Adapter Observer/event-forwarding impact, and Usage monitoring/visualization impact explicitly. Keep consumer implementation rules outside this skill. If proving a lifecycle guarantee requires consumer implementation tests, identify the owning repository and validation still required; do not expand an Abstractions-only task into consumer edits. Use the corresponding repository-specific plugin when available for authorized implementation work.

For deliberate semantic changes, update Abstractions source, tests, root `AGENTS.md`, and applicable migration guidance. Update affected consumer documentation only within authorized repository scope; otherwise report required follow-up, including Adapter-owned public documentation. Do not silently reconcile conflicting consumer assumptions.

## Report

State the contract change, optional-interface compatibility, nullable semantics, retry stability semantics, terminal-event semantics, Core impact, Adapter impact, Usage impact, tests/validation, and migration impact.
