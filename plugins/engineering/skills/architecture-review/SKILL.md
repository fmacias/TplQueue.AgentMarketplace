---
name: architecture-review
description: Review proposed or implemented changes that cross module boundaries, change dependencies or references, move public or internal responsibilities, introduce abstractions or components, or may alter layering. Also use for explicit architecture-review requests. Assess against repository-declared architecture and design intent.
---

# Review architecture

Analyze and report by default. Preserve established architecture and do not silently redesign the system. Derive architecture rules from repository evidence; do not impose an assumed architecture or invent module rules.

## Reconstruct the intended architecture

1. Identify the proposed change or implementation diff, affected modules, and repository boundaries. Follow applicable review prerequisites, including staged review when required for working-tree changes; do not stage changes automatically.
2. Read applicable common, repository, and nearest scoped `AGENTS.md`, root README, architecture and source-of-truth design documents, relevant interfaces/contracts, and available component, sequence, and activity diagrams. Inspect source structure, project references, and relevant callers and implementations.
3. Establish intended responsibilities, dependency directions, layering, public/internal boundaries, and integration flows before judging the change. Identify which documents are authoritative. Separate explicit rules from inferred conventions and observed implementation; existing code is evidence, not automatic proof of intended design.
4. Record missing or conflicting evidence. Do not silently reconcile contradictory documents or turn an inferred convention into a mandatory rule. If the task already resolves a conflict, apply that decision; otherwise surface the decision needed while continuing analysis that does not depend on it.

Use diagrams as architectural evidence when available:

- Component diagrams describe static dependencies and boundaries.
- Sequence diagrams describe integration interactions.
- Layered activity diagrams describe event/workflow responsibilities across layers.

Compare the relevant diagrams with contracts and source. Distinguish a stale diagram from an implementation violation when evidence permits; do not assume either is correct merely because it exists.

## Assess the change

Trace changed dependencies and responsibilities across the affected boundaries. Check for:

- Circular dependencies, reversed dependency direction, and layer violations.
- Public exposure of internal implementation types.
- Responsibility leakage between modules.
- Duplicated or parallel architecture and unnecessary coupling.
- Abstractions without a demonstrated need.
- Dependencies introduced for convenience rather than the declared design intent.

Relate each concern to a repository rule or concrete consequence. Do not treat every new abstraction, reference, or cross-module interaction as a defect. For proposals, state which effects are predicted; for implemented changes, identify the supporting code and affected consumers.

Classify apparent defects as a local implementation violation, documentation inconsistency, or genuine architecture limitation. If the evidence does not support a classification, explain the uncertainty rather than inventing a rule.

## Respect the design boundary

Prefer bounded corrections consistent with the established design. Make fixes only when explicitly authorized within the task's scope. If resolution requires broad architectural redesign, stop before invasive changes, explain the issue and consequence, and propose the safest bounded next step, such as resolving a contract ambiguity or seeking a focused design decision. Do not expand into unrelated modules or repositories.

## Report findings

For each actionable finding, provide:

- The architecture rule evaluated and its source, or the explicit uncertainty.
- The violation or risk and affected code, contract, or diagram location.
- The concrete consequence for boundaries, consumers, or behavior.
- The safest recommendation.
- Whether the issue is local, documentation-related, or requires broader design work.

Prioritize by impact and avoid manufacturing findings. State the scope and evidence examined, checks performed, unresolved questions, and material limitations. Say explicitly when no actionable architecture findings remain.
