---
name: validate-job-monitor-interaction
description: Validate TplQueue.Usage JobMonitor and Blazor monitor browser interactions, JavaScript interop, rendering, frontend state, and circuit lifecycle using the dedicated frontend/browser harnesses alongside relevant .NET checks.
---

# Validate JobMonitor interaction

Complement engineering's `testing-dotnet` and `validate-dotnet-change` when available and relevant. Keep this workflow focused on interactive monitor evidence rather than generic .NET validation.

## Discover current validation entry points

Read applicable parent instructions, the target root `AGENTS.md` or `Agents.md` and delegated/scoped instructions, the maintained Blazor frontend architecture guide, `tools/TplQueue.JobMonitor` documentation and test scripts, Blazor monitor test/harness documentation, and affected NUnit/integration tests.

Discover commands and arguments from the current repository. Do not freeze commands in this skill when the root Usage instructions do not declare them. The target scripts, tool documentation, and tests remain authoritative; surface conflicting or stale instructions.

## Choose evidence for the changed behavior

The current validation surfaces are complementary:

| Surface | Evidence |
| --- | --- |
| NUnit tests | Relevant .NET contract, projection, and integration behavior. |
| Standalone Node/browser JobMonitor harness | Frontend model checks and actual component interaction/rendering. Distinguish Node-only results from browser results. |
| Debug-only Blazor circuit/browser harness | Interactive integration, JS interop, and circuit/component lifecycle. |
| HTTP smoke/prerender checks | HTTP and static/prerender behavior, not interactive browser acceptance. |

Use interactive harness coverage when a change depends on browser interaction, JavaScript interop, frontend state, Blazor circuit lifecycle, or rendering behavior. A passing HTTP smoke test cannot substitute for a failing or unexecuted interactive check.

Test the changed contract, projection, and UI lifecycle boundaries relevant to the task. Verify actual feature behavior rather than only static markup. Do not expand testing into unrelated frontend functionality without a reason.

Validate maintained source assets through the normal synchronization workflow. Do not repair failures by editing ignored/generated sample `wwwroot` copies.

## Locate failures by evidence

Distinguish backend execution, Observer/projection, snapshot transport/interop, frontend model, visualization/layout, and browser lifecycle failures. Report where evidence places the failure and what remains uncertain; do not hide an interactive failure behind passing HTTP/static checks.

If the environment cannot run the required browser or harness, report that limitation and the unverified behavior explicitly. Successful .NET or Node-only tests do not establish the missing interactive evidence.

## Report

State the validation surface used, actual command/harness, behavior checked, result, environment/browser limitations, skipped or blocked interactive checks, distinction between HTTP/static and interactive evidence, and remaining risk.
