---
name: refactor-code
description: Refactor existing code when the human explicitly requests refactoring, cleanup, simplification, or maintainability improvement without primarily requesting a new feature. Preserve intended behavior and respect repository and module boundaries.
---

# Refactor code

Improve existing code with small, safe, understandable changes. Assume professional development competence; use repository policy and the requested outcome to guide decisions.

## Establish scope and review first

1. Confirm the requested repositories, modules, and intended improvement from the task and available context. Ask only when a material boundary remains unclear. Inspect existing changes and preserve unrelated human work.
2. Read applicable common, repository, and nearest scoped `AGENTS.md` instructions and relevant repository documentation. Retrieve architecture rules, invariants, compatibility targets, and validation commands there; do not assume repository-specific conventions.
3. Apply code-review analysis before editing. Follow repository review prerequisites, including staged review when required; stop and report unmet prerequisites without staging changes automatically. Inspect relevant configuration, affected public and internal code, consumers, and tests. Identify existing behavior, defects, compatibility risks, and the smallest safe change.
4. Respect established architecture, naming, layering, and design intent. Evaluate SOLID, DRY, KISS, YAGNI, separation of concerns, validation, readability, and maintainability pragmatically; assess concurrency, async, and serialization risks where relevant.

## Keep boundaries explicit

Do not modify dependent repositories unless the task explicitly permits cross-repository work or a blocking dependency requires broader scope and the human authorizes it. Existing authorization applies only to its stated scope.

If a dependency defect makes a safe refactor impossible, stop the affected work and expansion of scope. Explain the blocker and required dependent change; request broader authorization only if it is missing. Do not conceal the defect behind a local workaround. Continue independent in-scope work only when it remains safe and useful.

## Make proportionate changes

Appropriate actions within the requested scope include:

- Adding guard clauses or argument validation.
- Simplifying control flow and extracting private helpers.
- Removing dead private code after checking relevant usages, including indirect uses.
- Improving XML documentation and internal names without affecting the public API.
- Reducing duplication and improving cohesion and readability.
- Correcting clearly incorrect local logic when inseparable from the requested refactor, subject to the behavior boundary below.

Preserve intended externally observable behavior unless the task explicitly includes a bug fix. Check effects on outputs, exceptions, side effects, ordering, timing contracts, and public API compatibility where relevant. Guard clauses and logic corrections are not automatically behavior-preserving: if they would alter the intended contract, report the issue and obtain authorization before including that change.

Avoid speculative abstractions, unnecessary interfaces, premature extensibility, and unrelated architectural redesign. Do not change public APIs without an explicit task-related reason; explain compatibility implications and confirm scope if the change exceeds existing authorization. Defer separable defects or broader design issues rather than silently expanding the refactor.

## Validate and report

After modifications, apply available testing and validation Skills that match the changed scope. If none are available, follow `AGENTS.md` and repository documentation directly. Add or update tests where needed to verify preserved behavior or an explicitly authorized bug fix; follow local test conventions and required validation order. Do not remove valid coverage to make checks pass.

Review the final diff for unintended behavior or scope changes. Run relevant repository checks and distinguish successful validation from checks that failed or could not run.

Report what was refactored, why, the intended behavior preserved, any authorized behavior changes, tests and validation performed, and blockers or intentionally deferred issues. Include affected locations when useful and explain material validation limitations.
