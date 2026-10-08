---
name: validate-dotnet-change
description: Validate completed .NET code changes, refactoring, or implementation when validation is required. Use the repository's actual build, test, local packaging, and package-consumer workflow and report evidence and remaining gaps.
---

# Validate a .NET change

Validate the affected behavior and deliverables using repository-supported commands. Keep the work proportional to the change and report only what execution evidence establishes.

## Discover the real workflow

- Read applicable common, repository, and nearest scoped `AGENTS.md`, relevant repository documentation, scripts, project configuration, and CI definitions. Identify actual commands, working directories, prerequisites, framework targets, and dependency order.
- Map the change to affected projects, unit tests, packages, and integration or consumer checks. Determine whether packaged output is part of the task and which checks require fresh packages.
- Use commands and options documented or defined by the repository. Do not invent commands or assume a generic toolchain invocation replaces the supported workflow. If required commands cannot be established, report the missing workflow information and seek clarification when needed.
- Avoid expensive or unrelated validation without a concrete reason. Complete required repository checks; expand beyond them only when failures, dependencies, or unresolved risks justify it.

## Execute applicable phases

Prefer this order, respecting repository prerequisites and documented dependencies:

1. Build affected projects.
2. Run relevant unit tests.
3. Pack locally through the supported packaging mechanism when packaged output is part of the change.
4. Run applicable integration or package-consumer tests that depend on those packaged outputs.

For package-related tasks, verify that consumer checks actually resolve the newly produced package through the repository's supported mechanism. Source/project-reference tests do not replace package-consumption validation. Check for stale package resolution using available restore, artifact, or test evidence; disclose uncertainty if the consumed artifact cannot be confirmed.

If a prerequisite fails, do not treat checks against stale outputs as validation of the current change. Run independent checks when still meaningful, and mark dependent checks as blocked or skipped. Local packaging does not authorize publishing packages or changing unrelated consumer repositories.

## Interpret failures and gaps

- Record each executed command, relevant working directory or scope, and result. Distinguish passed, failed, skipped, blocked, and not applicable phases.
- If a step cannot run, name it and explain the concrete reason. Never claim success for an unexecuted or incomplete step.
- Where evidence permits, distinguish failures caused by the current change, known pre-existing failures, and environment/tooling failures. Support classification with diagnostics or available baseline evidence; label uncertain causes as undetermined rather than guessing.
- Keep validation separate from unrequested implementation changes. Report defects and blockers; make fixes only within the authorized task. Repeat checks after relevant fixes or environment corrections, not merely to obtain a passing run.

## Report validation

Provide a concise record, using a table when useful:

| Command or check | Working directory / scope | Result |
| --- | --- | --- |
| Actual command, or named phase if skipped | Affected project or directory | Outcome and brief evidence or skip reason |

Include skipped checks and reasons, failure classification where supported, and remaining risk. For package-related work, state whether consumption of the current packaged output was verified. Do not equate a successful build or unit-test run with complete validation when required packaging or consumer checks remain outstanding.
