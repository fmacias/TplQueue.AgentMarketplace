---
name: implement-feature
description: Implement a bounded feature, bug fix, added behavior, or explicitly defined implementation task while preserving existing architecture, compatibility, and repository conventions.
---

# Implement a feature or bug fix

Deliver the requested behavior with the smallest coherent change. Assume professional engineering competence; keep implementation bounded by the human's request.

## Understand before editing

1. Identify the requested outcome, repository/module scope, and observable acceptance criteria. Inspect existing changes and preserve unrelated human work.
2. Read applicable common, repository, and nearest scoped `AGENTS.md`, the affected repository's `README`, and relevant architecture documentation. Inspect the current implementation, consumers, tests, and project configuration. Derive compatibility targets, runtime invariants, testing policy, and validation commands from those sources rather than assuming project-specific rules.
3. Apply review/refactoring discipline: examine responsibilities, duplication, complexity, validation, side effects, failure paths, testability, and compatibility risks before choosing a change. Follow applicable repository prerequisites.
4. Infer the safest interpretation when intent can reasonably be recovered from code and documentation. Resolve ordinary implementation details without unnecessary questions. Ask only when ambiguity materially changes behavior, compatibility, scope, or correctness.

## Implement within the existing design

- Preserve architecture, naming, conventions, layering, and module boundaries. Keep public APIs and backward compatibility where practical; make necessary changes explicit and within the requested scope.
- For a bug, identify the root cause and reproduce or cover the failure through tests where practical. Correct the cause with the smallest coherent fix; do not merely mask the visible symptom.
- For a feature, integrate through existing abstractions. Allow extensibility justified by the requested behavior, without creating a parallel ad-hoc architecture, speculative generalization, or unnecessary interfaces.
- Keep supporting refactors necessary and local. Do not silently redesign the library or unrelated modules. Modify dependent repositories only when necessary and authorized by the task's scope; report blocking dependency defects and required changes if broader authorization is missing.
- Update relevant documentation to reflect changed behavior, following repository conventions.

## Check concurrency when affected

Apply the [concurrency-review workflow](../review-concurrency/SKILL.md) when changes affect shared state, async execution, scheduling, cancellation, retries, callbacks, or ordering. Recheck the implemented paths before completion.

If that Skill is unavailable in the destination environment, perform the equivalent focused review: establish the concurrency model and local invariants; trace reachable shared state, synchronization, completion/error ownership, cancellation lifetimes, retry effects, ordering guarantees, and shutdown/disposal races. Explain concrete failing interleavings and address in-scope defects. Obtain repository-specific semantics from local instructions and architecture documentation.

## Test and validate

1. Apply applicable testing policy and available testing Skills. Add or adapt tests belonging to the changed behavior, using test-first development where repository policy requires it. Cover relevant successful, boundary, invalid-input/state, and failure paths; include regression coverage for bug fixes and concurrency boundaries when affected.
2. Prefer preserving existing tests. Do not delete or weaken tests merely because implementation changes make them inconvenient. If a test is genuinely obsolete or invalid, explain and justify its removal or replacement separately, preserving useful coverage.
3. Follow repository validation instructions and applicable validation Skills. Build affected projects, run relevant tests, package locally when applicable, and run integration checks, respecting dependencies and the prescribed order. Derive commands from repository documentation; do not hard-code a toolchain.
4. Inspect the final diff for scope, compatibility, and unintended behavior changes. Resolve relevant validation failures. If tests or checks cannot be added or run, report the concrete blocker and remaining validation gap; do not claim complete verification.

## Report completion

Describe the implemented behavior, important design decisions, files or areas changed, tests added or changed, and validation performed with outcomes. State limitations, blockers, and deferred work explicitly, including any justified test removals or compatibility changes.
