---
name: testing-dotnet
description: Add or update scoped .NET/C# tests when implementation, bug fixing, or refactoring requires test changes, or when the human explicitly asks for tests. Follow repository testing policy and verify the behavior relevant to the requested change.
---

# Test .NET changes

Add or update only tests relevant to the requested change. Assume professional .NET testing competence; do not expand the suite for generic coverage improvement.

## Establish scope and conventions

- Read applicable common, repository, and nearest scoped `AGENTS.md`, relevant repository documentation, test configuration, and existing tests. Obtain test project paths, commands, framework versions, and prerequisites from those sources.
- Identify the changed behavior, intended contract, and relevant unit or integration boundaries. Choose tests that verify observable behavior rather than mirror implementation details.
- Default to NUnit, Moq where mocking is appropriate, and Arrange / Act / Assert structure. Repository instructions declaring other tooling take precedence; follow the established suite rather than introducing a competing framework solely to satisfy these defaults.
- Respect repository test-first policy. For bug fixes, prefer a failing regression test demonstrating the defect before or alongside the fix when practical. Confirm that the failure exposes the intended defect rather than an unrelated setup problem.

## Select meaningful coverage

Cover the following where applicable to the changed behavior:

- Valid behavior.
- Boundary and edge cases.
- Invalid arguments.
- Invalid state transitions.
- Exception and failure paths.

Keep integration tests readable, representative of real component composition, and focused on meaningful integration boundaries. Use mocks where isolation helps establish the contract; avoid mocking away the interaction an integration test is meant to verify.

For concurrency tests, use controllable synchronization primitives, gates, or completion signals to establish relevant interleavings. Prefer deterministic assertions; do not use arbitrary sleeps as the primary assertion mechanism. Exercise race and cancellation boundaries relevant to the change. Use bounded waits to detect hangs, and clean up tasks and resources so failures do not contaminate other tests.

## Preserve existing coverage

Preserve existing tests. Do not delete or weaken them merely because implementation changes make them inconvenient. Remove a test only when it is objectively invalid, obsolete because supported behavior was intentionally removed, or replaced by demonstrably better coverage.

Explain every removal or replacement explicitly: identify the invalid assumption or intentionally removed behavior, or show how the replacement improves coverage. Distinguish a broken test from a production regression before changing assertions. Keep test changes within the authorized implementation, refactor, or explicit testing scope.

## Execute and report

Run relevant repository test commands and required prerequisites. Start with checks that establish the changed behavior, then complete the validation required by repository policy. Investigate relevant failures; distinguish failures introduced by the change from existing failures and environment blockers. Do not report unexecuted tests as passing.

Report:

- Tests added or changed and the behavior each proves.
- Tests removed or replaced and the justification.
- Relevant tests intentionally not added and why.
- Executed test commands and their results, including failures or checks that could not run.
