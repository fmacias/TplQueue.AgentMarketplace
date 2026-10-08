---
name: align-cross-repository-documentation
description: Coordinate documentation changes affecting multiple repositories or information mirrored or referenced across repository boundaries, including navigation, publishing instructions, licensing text, and repository-boundary explanations.
---

# Align cross-repository documentation

Keep directly affected documentation consistent with its documented authority while preserving intentional repository differences. Limit edits to the requested alignment.

## Establish the affected scope

1. Identify every directly affected repository, including sources and consumers of mirrored or referenced information. Follow relevant navigation, sync, and publishing references far enough to understand the boundary without expanding into unrelated repositories.
2. For each directly affected repository, read the root `AGENTS.md` or `Agents.md` and root `README.md` before editing. Read both instruction files if they are distinct and both exist. Then read any more-specific instruction file for the affected surface, along with applicable common instructions.
3. Inspect existing changes and preserve unrelated human work. Record missing or inaccessible repositories or required documents; do not assume their contents or claim complete alignment without examining them.

## Resolve authority and contradictions

- Identify the documented source-of-truth model: which repository or document owns each relevant statement, which surfaces mirror or reference it, and how changes propagate through sync or publishing. Do not infer authority solely from recency, matching wording, or repository names.
- Compare the requested change with current documentation before editing. Record contradictions with affected locations and conflicting statements. Distinguish accidental drift from real differences in repository responsibility, licensing, publication scope, or supported behavior.
- Never silently normalize contradictory repositories. If an explicit human instruction already resolves a contradiction, follow it within the authorized scope and document the alignment; do not request the same decision again.
- If intent remains genuinely unresolved, surface the contradiction and available choices, such as aligning to a specified authority or preserving an intentional distinction. Ask for the decision needed before dependent edits; continue independent work when safe. Do not invent a unified policy or treat silence as a decision.

## Apply coordinated edits

Update the relevant surfaces together, as applicable to the resolved decision:

- Root README and affected documentation pages.
- `AGENTS.md` / `Agents.md` instructions.
- Navigation, indexes, and cross-repository links.
- Sync and publishing instructions.
- Repository-boundary explanations.
- Related licensing and publication guidance.

Follow the documented source and propagation workflow for generated or mirrored content. Preserve repository-specific wording where it expresses a real distinction. Do not broaden into unrelated cleanup, invent licensing terms, or change repository responsibilities beyond the requested decision. Editing publishing instructions does not itself authorize publication.

If an affected surface cannot be updated, identify the outstanding change and its consequence rather than presenting a partial alignment as complete.

## Validate and report

Review the diffs across affected repositories against the chosen source-of-truth decision. Check relevant relative paths, cross-repository links, navigation entries and anchors, and the consistency of sync/publishing instructions. Use repository-supported documentation or navigation checks where available; distinguish static inspection from commands actually executed. Report checks that could not run and why.

Report the repositories examined, contradictions found, source-of-truth decision applied, documents changed, unresolved or intentionally preserved differences, and validation/navigation checks performed with their results.
