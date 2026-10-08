---
name: review-csharp
description: Review C# changes when the human explicitly requests a code review, staged-change review, commit or diff review, or assessment of C# changes without broad implementation changes. Analyze and report by default; make fixes only when explicitly requested with the review.
---

# Review C# changes

Apply engineering policy and the review process below, assuming professional C# knowledge. Focus on defects, regressions, risks, and concrete maintainability problems.

## Establish scope and compatibility

1. Identify the requested repository, changes, and comparison base. Read applicable common, repository, and nearest scoped `AGENTS.md` instructions. Preserve repository-specific architecture, naming, layering, design intent, and public behavior.
2. For a current-working-tree review, inspect `git status --short`, `git diff --cached`, and `git diff` to verify that the relevant changes are staged and identify partially staged files. If repository policy requires staged review and relevant changes are unstaged, stop and report this before the formal review. Do not stage changes automatically. Otherwise, explicitly state which staged or unstaged changes are being reviewed. For an explicit commit or diff, inspect that requested snapshot; unrelated working-tree staging is not a prerequisite.
3. Before judging compatibility, inspect relevant project files, imported `.props`/`.targets`, solution-level settings, and SDK/language configuration. Resolve the applicable framework and language targets from those files and repository instructions, including multiple targets where present. If configuration conflicts with the intended targets, stop and report the inconsistency first. Do not assume or embed a particular framework or version.
4. Inspect surrounding code and all relevant public and internal services within the affected scope, including consumers, tests, and documentation needed to understand behavior. Review the selected snapshot consistently; do not attribute unrelated working-tree changes to a staged diff or commit.

## Evaluate the changes

- Apply SOLID, DRY, KISS, YAGNI, Separation of Concerns, Fail Fast, defensive programming, reasonable immutability by default, readability, and maintainability. Check thread safety, safe async usage, and serialization safety where relevant.
- Explicitly look for duplication, long or unnecessarily complex methods, missing validation or guard clauses, hidden side effects, poor responsibility separation, misleading naming, test gaps, documentation gaps, and backward-compatibility risks.
- Require static helpers to be stateless and avoid mutating instance or global state. Prefer internal visibility unless a public static API has a concrete justification.
- Keep SOLID analysis pragmatic: static factories may intentionally provide internal construction; not every internal implementation needs substitution or extensibility. Expect appropriate testability, including for internal and non-public services. Do not demand speculative abstractions.
- Assess relevant test coverage for valid paths, edge cases, invalid arguments or state transitions, and exception paths. Follow local test conventions. Check that relevant documentation, including XML comments, accurately describes behavior. Distinguish missing coverage from verified defects.

## Respect the review boundary

Review means analysis and reporting. Do not modify code, tests, or documentation, including minor corrections, unless the human explicitly requested review plus fixes. Keep any authorized fixes within that scope and follow repository validation requirements.

For major OCP, LSP, or other design problems requiring architectural change, report the affected design, explain the concrete consequence, and propose the safest next step. Stop before invasive changes; do not silently redesign or broaden the task.

## Report findings

Group findings in this order, omitting empty categories:

1. Critical issues
2. Design issues
3. Maintainability issues
4. Test gaps
5. Optional improvements

For each finding, identify the affected file and line or symbol when available, describe the evidence and triggering conditions, explain the consequence, and suggest a proportionate next step. Separate optional improvements from required corrections; do not manufacture findings to fill categories.

State the reviewed scope, any assumptions or unresolved questions, and validation performed or not performed. If no actionable findings remain, say so explicitly and mention any material review limitations.
