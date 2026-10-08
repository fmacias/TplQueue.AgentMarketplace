---
name: document-csharp-api
description: Create or improve precise English XML documentation when new C# APIs require documentation, substantially modified APIs need documentation updates, or the human explicitly requests API or code documentation cleanup. Keep edits within the affected scope.
---

# Document C# APIs

Write concise, useful English documentation consistent with actual behavior. Assume professional C# knowledge; do not turn API comments into tutorials or verbose restatements of names.

## Establish semantics and scope

- Read applicable `AGENTS.md`, repository documentation conventions, and the affected API's implementation, contracts, tests, and relevant callers.
- Identify which new or modified APIs and existing comments belong to the requested scope. Preserve unrelated work and avoid unrelated documentation cleanup.
- Determine supported semantics from the available evidence. If code, tests, or documentation are ambiguous or contradictory, report the uncertainty rather than documenting an assumption as fact. Continue only with statements that can be established.

## Write or correct documentation

- Use English and appropriate XML documentation elements. Describe purpose, parameters, return values, exceptional behavior, and lifecycle or ownership semantics when they materially help the consumer.
- Keep parameter names, type references, return descriptions, and exception conditions aligned with the actual declaration and behavior. Explain meaningful constraints or outcomes instead of repeating the method or type name.
- Document only supported behavior. Do not invent guarantees about nullability, thread safety, ordering, cancellation, disposal, ownership, or exceptions that code, tests, and contracts do not establish.
- Keep public consumer documentation focused on the exposed contract. Use internal implementation comments for relevant rationale or mechanics; do not present incidental implementation details as public guarantees.
- Remove or correct misleading and outdated comments within the affected scope. Do not change runtime behavior merely to make a comment true; report defects requiring separate implementation work.

## Check and report

Review XML structure, parameter names, references, and consistency with the implementation. Run relevant repository-supported documentation/compiler checks when applicable; report checks that could not run. Do not add tests solely to validate comment wording.

Summarize the APIs documented, meaningful corrections, checks performed, and unresolved semantic questions.
