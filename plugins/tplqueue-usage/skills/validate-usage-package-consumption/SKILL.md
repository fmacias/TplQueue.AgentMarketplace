---
name: validate-usage-package-consumption
description: Validate TplQueue.Usage public package consumption, resolved package versions, and sample consumers while distinguishing standalone package validation from intentional workspace project-reference execution.
---

# Validate Usage package consumption

Complement engineering's `validate-dotnet-change`, `testing-dotnet`, and `review-public-api` when available and relevant. Use `validate-dotnet-change` for normal build/test execution discipline; keep this skill focused on evidence of public package consumption.

## Establish the validation mode

Read applicable parent instructions, the target root `AGENTS.md` or `Agents.md` and delegated/scoped instructions, `Directory.Build.props`, `NuGet.config`, relevant project files, build/test/coverage scripts, and affected sample/test projects.

Treat Usage as the public package-consumption and validation surface. Prefer package-consumer evidence over private-source assumptions. Determine whether this run validates standalone package consumption or intentional workspace project-reference mode. These modes are separate and must not be treated as equivalent.

Resolve the intended package line from the shared `TplQueuePackageVersion` property in `Directory.Build.props`, accounting for script or build overrides. Do not hard-code a version in this skill. Preserve the shared property so build, test, and coverage overrides select one consistent package line.

Treat all sample projects as repository-local unless the target repository explicitly establishes another current arrangement. Preserve intentional workspace source-reference switching as a separate validation mode.

## Validate through supported entry points

The current root workflow uses `./build.ps1`, `./test.ps1`, and `./coverage.ps1`. Inspect their current arguments and behavior before invocation; do not invent undocumented options. The target scripts and configuration are authoritative.

For package-consumption work:

- Verify the references the consumer actually resolves, using evaluated build/restore output and resolved dependency evidence as appropriate. Declared package references alone do not prove the active reference mode.
- Distinguish package references from project/source references and verify that the intended package line is active.
- Check for stale or unintended package/output resolution before accepting results.
- Identify package-consumer failures separately from source-build failures. A successful project-reference run does not establish that public package consumption works.

Do not change package dependencies merely to perform documentation-only work.

## Respect defect ownership

If validation exposes a defect owned by Adapter, Abstractions, or Core, name the owner and the required dependency explicitly. Do not implement that fix in another repository during a Usage-only task. Report the resulting validation limit.

## Report

State the reference mode validated, active package-version source and effective version/override, affected sample or consumer, commands executed, package resolution evidence, test/coverage outcome, blocked or skipped checks, and any related repository defect. Keep source-build success distinct from package-consumer evidence.
