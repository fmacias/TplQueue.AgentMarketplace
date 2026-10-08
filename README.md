# TplQueue Agent Marketplace

A repository marketplace containing **Engineering**, with portable C# and .NET engineering workflows, and **TplQueue Adapter**, with seven instruction-only workflows specific to TplQueue.Adapter. The skills obtain authoritative architecture, compatibility targets, runtime invariants, and validation commands from the repository in which they are used.

## Package layout

- [Marketplace catalog](.agents/plugins/marketplace.json): the `tplqueue-agent-marketplace` marketplace, with `engineering` followed by `tplqueue-adapter`.
- Portable manifests: [Engineering](plugins/engineering/plugin.json) and [TplQueue Adapter](plugins/tplqueue-adapter/plugin.json), each canonical for its plugin's identity and version.
- Codex compatibility manifests: [Engineering](plugins/engineering/.codex-plugin/plugin.json) and [TplQueue Adapter](plugins/tplqueue-adapter/.codex-plugin/plugin.json), with matching metadata, skill discovery, and OpenAI presentation.
- Skills: [eleven Engineering workflows](plugins/engineering/skills/) and [seven Adapter workflows](plugins/tplqueue-adapter/skills/).

Marketplace sources `./plugins/engineering` and `./plugins/tplqueue-adapter` resolve relative to this repository root. Skills live at each plugin root under `skills/`. OpenAI metadata is supplied by each compatibility manifest; the portable manifests intentionally have no `extensions.com.openai` override.

## Included skills

### Engineering

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

### TplQueue Adapter

| Skill | Purpose |
| --- | --- |
| `review-adapter-architecture` | Review package boundaries, facade responsibilities, and dependencies. |
| `modify-adapter-public-contracts` | Change public contracts with compatibility and migration impact in view. |
| `modify-adapter-retry-policy` | Preserve policy resolution and root/queue inheritance semantics. |
| `modify-adapter-observer-flow` | Maintain subscription-based Job lifecycle notifications. |
| `modify-adapter-cacheq-lifecycle` | Review cache transitions, queue completion, acknowledgments, and disposal. |
| `modify-adapter-di-registration` | Preserve configuration-time builders and independent registration snapshots. |
| `update-adapter-public-documentation` | Maintain Adapter's English/German documentation and migration guidance. |

Adapter skills complement Engineering and read the target checkout's instructions, source, tests, and documentation. Adapter package subcomponents remain within `tplqueue-adapter`; additional skills require a distinct reusable workflow established by package-level instructions. TplQueue.Usage remains a separate consumer repository and future `tplqueue-usage` plugin.

## Use locally

From this repository root, register the marketplace:

```powershell
codex plugin marketplace add .
codex plugin marketplace list
```

In the Codex app, open the plugin directory, select **TplQueue Agent Marketplace**, and install **Engineering** and/or **TplQueue Adapter**. Start a new thread to use the installed skills. For example, request `$review-csharp` for staged C# changes or `$review-adapter-architecture` for an Adapter architecture review.

This repository uses the marketplace identifier `tplqueue-agent-marketplace`; registration associates that identifier with this checkout. If another marketplace already uses that identifier, resolve the naming conflict before registering it.

## Maintain the package

Keep the portable and compatibility manifests' shared identity, version, description, author, keywords, and declared license synchronized. Validate plugin metadata and each changed skill before distribution. When refreshing an installed local plugin, use the plugin-creator update/reinstall workflow; if its cachebuster helper changes the compatibility manifest version, synchronize the portable version before reinstalling.

Local registration makes this checkout available to Codex. It does not publish the plugins to the public directory. The repository is covered by the [MIT license](LICENSE); no public repository URL is declared in the plugin manifests.

Package layout, marketplace conventions, and local registration follow the [official OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins).
