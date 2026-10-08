---
name: review-public-api
description: Review .NET changes affecting public classes, interfaces, members, DTOs or contracts, visibility, public static APIs, or signatures used across modules and packages. Identify unnecessary exposure, compatibility risks, and architectural leakage under declared repository policy.
---

# Review public .NET APIs

Analyze and report by default; modify code only when fixes are explicitly requested. Minimize public API changes and preserve backward compatibility where practical and where repository instructions require it.

## Establish the changed contract and policy

- Identify the requested diff or commit and follow applicable repository review prerequisites, including staged review when required. Do not stage changes automatically.
- Read applicable common, repository, and nearest scoped `AGENTS.md`, relevant architecture/API documentation, and project configuration. Determine the declared target frameworks and any compatibility, versioning, serialization, or deprecation/obsolete policy.
- Compare the previous and proposed API surface. Identify changed public types, members, interfaces, DTOs, visibility, static APIs, and signatures used across module or package boundaries. Establish why each change is needed from the task and implementation evidence; state uncertainty rather than inventing intent.
- Inspect relevant implementations and available consumers to understand architecture, usage, and intended behavior. Distinguish observed consumer impact from potential impact on consumers unavailable for inspection.

## Evaluate exposure and consumer risk

- Check whether public visibility is genuinely necessary for the intended consumers. Flag unnecessary exposure and internal implementation types or responsibilities leaking through public contracts.
- Keep static helpers internal unless a public static API has a clear justification. Do not automatically recommend reducing visibility on an existing public API without assessing the resulting consumer risk.
- Check naming and semantic clarity of exposed contracts, including whether signatures and documentation communicate their intended responsibilities and behavior.
- Assess concrete risks to existing consumers from removals, visibility reductions, signature or contract changes, and changed observable behavior. Explain the affected usage and consequence rather than declaring broad compatibility guarantees.
- Prefer the smallest API change that meets the requested behavior and respects established architecture and module boundaries. Report broader design issues and the safest next step; do not silently redesign contracts.
- Determine whether relevant consumer tests, public API documentation, examples, or serialization tests need to change. Distinguish missing evidence or coverage from a demonstrated defect.

## Keep advanced policy repository-dependent

The shared engineering policy does not yet define a complete binary-compatibility, semantic-versioning, or deprecation policy. Consult explicit repository policy before making advanced compatibility or versioning judgments.

Do not invent SemVer rules, binary compatibility guarantees, interface evolution policies, serialization-version guarantees, or mandatory deprecation/obsolete strategies. Do not prescribe a version bump or compatibility promise without a declared basis. Concrete technical risks may still be reported with evidence, while the acceptable compatibility tradeoff remains a repository/policy decision.

If required policy is absent or contradictory, identify the specific decision needed and its consequence. Continue evidence-based review where possible; do not treat missing policy as permission to assume a guarantee or manufacture a violation.

## Report findings

Group actionable findings under these concerns, omitting empty categories:

- Breaking or compatibility risks.
- Unnecessary exposure.
- Architecture leakage.
- Consumer impact.
- Documentation and test implications.
- Questions requiring repository-specific policy.

For each finding, identify the affected file and line or API symbol when available, the evidence or consumer scenario, the consequence, and a proportionate next step. Avoid duplicating one issue across categories. State the reviewed scope, validation performed, and material limits; say explicitly when no actionable findings remain.

Policy follow-up: Enhance this Skill when an explicit shared API-compatibility policy is defined, referencing that policy rather than introducing independent guarantees here.
