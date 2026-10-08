# TplQueue Agent Marketplace

A repository marketplace containing the **Engineering** plugin: portable C# and .NET engineering workflows. The skills obtain project-specific architecture, compatibility targets, runtime invariants, and validation commands from the repository in which they are used.

## Package layout

- [Marketplace catalog](.agents/plugins/marketplace.json): the `tplqueue-agent-marketplace"` marketplace, using the plugin-creator default identifier, with one `engineering` entry.
- [Portable plugin manifest](plugins/engineering/plugin.json): canonical plugin identity and version.
- [Codex compatibility manifest](plugins/engineering/.codex-plugin/plugin.json): matching identity, skill discovery, and OpenAI presentation metadata.
- [Skills](plugins/engineering/skills/): eleven reusable workflows.

The marketplace source `./plugins/engineering` resolves relative to this repository root. The skills live at the plugin root under `skills/`. OpenAI metadata is supplied by the compatibility manifest; the portable manifest intentionally has no `extensions.com.openai` override.

## Included skills

| Skill | Purpose |
| --- | --- |
| `review-csharp` | Review scoped C# changes against engineering and repository policy. |
| `review-concurrency` | Review async, shared state, cancellation, retries, and ordering. |
| `review-public-api` | Review API exposure and declared compatibility requirements. |
| `architecture-review` | Assess boundaries, dependencies, layering, and design intent. |
| `refactor-code` | Improve code while preserving intended behavior and scope. |
| `implement-feature` | Implement a bounded feature or root-cause bug fix. |
| `testing-dotnet` | Add or adapt relevant unit and integration tests. |
| `validate-dotnet-change` | Validate builds, tests, packages, and package consumers. |
| `document-csharp-api` | Write precise English XML documentation. |
| `align-cross-repository-documentation` | Coordinate documentation across repository boundaries. |
| `summarize-staged-commit` | Generate commit text from staged changes without editing them. |

## Use locally

From this repository root, register the marketplace:

```powershell
codex plugin marketplace add .
codex plugin marketplace list
```

In the Codex app, open the plugin directory, select **Personal**, and install **Engineering**. Start a new thread to use its skills. For example, request `$review-csharp` for staged C# changes or `$testing-dotnet` for tests belonging to a scoped implementation.

This repository uses the default marketplace identifier `personal`; registration associates that identifier with this checkout. If another marketplace already uses that identifier, resolve the naming conflict before registering it.

## Maintain the package

Keep the portable and compatibility manifests' shared identity, version, description, author, and keywords synchronized. Validate plugin metadata and each changed skill before distribution. When refreshing an installed local plugin, use the plugin-creator update/reinstall workflow; if its cachebuster helper changes the compatibility manifest version, synchronize the portable version before reinstalling.

Local registration makes this checkout available to Codex. It does not publish the plugin to the public directory. No license or public repository URL is declared because neither has been established for this repository.

Package layout, marketplace conventions, and local registration follow the [official OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins).
