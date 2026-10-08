---
name: update-adapter-public-documentation
description: Update public TplQueue documentation owned by TplQueue.Adapter, including docs/en, docs/de, README, API migration and configuration guidance, and documentation navigation or publishing configuration.
---

# Update Adapter public documentation

Complement engineering's `align-cross-repository-documentation`, `document-csharp-api`, and `review-public-api` when available and relevant. Keep this workflow focused on Adapter documentation ownership.

## Establish authoritative sources

1. Read applicable parent instructions, target root `AGENTS.md`, `docs/Agents.md`, root `README.md`, affected English/German documentation, and nearest package instructions where relevant. Inspect source and tests when describing runtime behavior.
2. Treat `docs/Agents.md` as the authoritative detailed instruction set for end-user documentation. The target instructions, documentation, source, tests, and public contracts remain authoritative; surface contradictions rather than inventing current behavior.
3. For public behavior or API migration changes, inspect `docs/en/operations/api-migration.md` and affected end-user pages. Keep migration guidance consistent with current source, tests, and public contracts.

## Preserve ownership and publishing

- Keep `docs/en/` and `docs/de/` as the publishable source-of-truth language trees. Update affected content according to the target documentation instructions and report each language's impact.
- `docs/Agents.md` is instructions only; do not mirror it into public site output.
- Keep root `README.md` concise as the repository/package entry point. Do not copy detailed writing policy from `docs/Agents.md` into root `AGENTS.md`.
- Public TplQueue documentation continues to be published from Adapter's language trees. Do not transfer publishing responsibility unless the explicit task changes that architecture. Editing publishing configuration does not itself authorize publication.
- Preserve public terminology: Job/JobRoot, DataJob/DataJobRoot, ParallelQ, FifoQ, and CacheQ. Do not present planned or speculative behavior as current behavior.

## Coordinate within repository boundaries

When Core/Abstractions documentation must align, use `align-cross-repository-documentation` when available. Identify authority and unresolved differences explicitly; do not silently reconcile contradictions or modify related repositories outside the requested scope.

Identify Usage sample/integration documentation impact without transferring public documentation ownership to Usage. Usage remains a separate consumer repository and future `tplqueue-usage` plugin; use it when available for Usage-specific work. Keep detailed Usage documentation, UI, composition, and execution procedures out of Adapter skills.

Package-specific documentation remains Adapter documentation. Do not create a plugin merely because a package has documentation, or automatically create one skill per package. Additional `tplqueue-adapter` skills require a distinct reusable package-level `AGENTS.md` workflow not adequately covered by engineering, root Adapter instructions, or the existing seven skills. Keep them here unless independently useful enough to justify another plugin.

## Validate and report

Run applicable target documentation/navigation checks and inspect affected publishing configuration and links. Report source files affected, English tree impact, German tree impact, migration documentation impact, Core/Abstractions alignment impact, Usage impact, and publishing/navigation validation actually performed or still required.
