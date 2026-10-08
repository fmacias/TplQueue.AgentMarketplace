---
name: summarize-staged-commit
description: Inspect staged Git changes and produce a concise, human-readable commit message when the human asks for a commit message, commit text, a summary of staged changes for commit, or a conventional or humanized description of the staged diff.
---

# Summarize staged changes for commit

Produce a ready-to-paste commit message representing what the staged diff actually implements. This workflow is read-only: do not edit files, stage or unstage changes, create commits, or otherwise modify the repository.

## Inspect the staged snapshot

1. Identify the relevant Git repository and read applicable `AGENTS.md` instructions for commit conventions.
2. Inspect `git diff --cached --stat` and the complete `git diff --cached`, or equivalent index-based commands. Include staged additions, deletions, and renames. If nothing is staged, report that and stop; do not draft a message from unstaged or untracked work.
3. Use repository context only to interpret the staged changes. When surrounding file content is needed, read the index version with `git show :path/to/file` and committed history where useful. Do not use working-tree file contents to infer implemented changes: partially staged files may contain additional unstaged work.
4. Deduce the implemented concerns and their intent or result from the diff. Do not substitute the requested plan, conversation history, or expected future work for evidence of what is staged. For binary or otherwise opaque changes, describe only what can be established.

## Write the commit message

- Group related modifications by implemented concern rather than mechanically listing files.
- Write a concise, specific subject in human-readable language. Follow repository conventions; use Conventional Commits formatting when requested or required, without inventing issue identifiers or scopes.
- Add an explanatory body only when multiple meaningful aspects or necessary context warrant it. Explain the implemented result and supported rationale; avoid generic phrases such as "updated code" when a specific description is available.
- Do not claim behavior unsupported by the staged diff. Mention tests or documentation only when staged or necessary to understand the commit. Adding tests does not establish that they passed; do not invent execution results.
- If staged changes contain unrelated concerns, point them out separately and suggest splitting when useful. Do not fabricate a single shared purpose or change the index. If providing one message for the existing staged set, describe its distinct concerns honestly.

## Return ready-to-paste text

Put the commit subject and optional body in one plain-text code block, with a blank line between subject and body. Keep any scope warning or material uncertainty outside that block so the message itself is ready to paste. Do not add routine commentary when the message is sufficient.
