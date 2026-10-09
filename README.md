# TplQueue Agent Marketplace

A repository marketplace containing **Engineering**, with portable C# and .NET engineering workflows; **TplQueue Adapter**, with seven instruction-only Adapter workflows; **TplQueue Abstractions**, with three instruction-only contract workflows; and **TplQueue Usage**, with six instruction-only sample and consumer workflows. The skills obtain authoritative architecture, compatibility targets, runtime invariants, and validation commands from the repository in which they are used.

## Package layout

- [Marketplace catalog](.agents/plugins/marketplace.json): the `tplqueue-agent-marketplace` marketplace, ordered as `engineering`, `tplqueue-adapter`, `tplqueue-abstractions`, and `tplqueue-usage`.
- Portable manifests: [Engineering](plugins/engineering/plugin.json), [TplQueue Adapter](plugins/tplqueue-adapter/plugin.json), [TplQueue Abstractions](plugins/tplqueue-abstractions/plugin.json), and [TplQueue Usage](plugins/tplqueue-usage/plugin.json), each canonical for its plugin's identity and version.
- Codex compatibility manifests: [Engineering](plugins/engineering/.codex-plugin/plugin.json), [TplQueue Adapter](plugins/tplqueue-adapter/.codex-plugin/plugin.json), [TplQueue Abstractions](plugins/tplqueue-abstractions/.codex-plugin/plugin.json), and [TplQueue Usage](plugins/tplqueue-usage/.codex-plugin/plugin.json), with matching metadata, skill discovery, and OpenAI presentation.
- Skills: [eleven Engineering workflows](plugins/engineering/skills/), [seven Adapter workflows](plugins/tplqueue-adapter/skills/), [three Abstractions workflows](plugins/tplqueue-abstractions/skills/), and [six Usage workflows](plugins/tplqueue-usage/skills/).

Marketplace sources `./plugins/engineering`, `./plugins/tplqueue-adapter`, `./plugins/tplqueue-abstractions`, and `./plugins/tplqueue-usage` resolve relative to this repository root. Skills live at each plugin root under `skills/`. OpenAI metadata is supplied by each compatibility manifest; the portable manifests intentionally have no `extensions.com.openai` override.

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

Adapter skills complement Engineering and read the target checkout's instructions, source, tests, and documentation. Adapter package subcomponents remain within `tplqueue-adapter`; additional skills require a distinct reusable workflow established by package-level instructions. TplQueue.Usage remains a separate consumer repository with its own `tplqueue-usage` plugin.

### TplQueue Abstractions

| Skill | Purpose |
| --- | --- |
| `review-abstractions-architecture` | Review the shared contract boundary, responsibilities, and downstream coupling. |
| `modify-abstractions-public-contracts` | Change owned public contracts with compatibility, migration, and consumer impact in view. |
| `modify-execution-channel-observation` | Preserve optional diagnostic metadata, nullable channels, retry stability, and terminal-event semantics. |

Abstractions skills complement Engineering and discover contract ownership from the target checkout. Core, Adapter, and Usage remain separate repository boundaries; Adapter continues to own publishable public documentation. Additional Abstractions skills require a distinct reusable contract workflow or non-trivial invariant established by repository instructions, rather than one skill per type or folder.

### TplQueue Usage

| Skill | Purpose |
| --- | --- |
| `review-usage-sample-architecture` | Review module ownership, host composition, service lifetimes, and queue identity. |
| `validate-usage-package-consumption` | Verify public package consumption separately from workspace source references. |
| `modify-usage-simulation-runtime` | Preserve scheduling, host lifecycle, graph membership, and shared runtime ownership. |
| `modify-blazor-monitor-projection` | Maintain immutable snapshots, Observer semantics, DTO mappings, and JS interop. |
| `modify-job-monitor-visualization` | Preserve time/channel placement, enqueue history, collision behavior, and accessibility. |
| `validate-job-monitor-interaction` | Validate frontend and Blazor browser behavior with dedicated interactive harnesses. |

Usage skills complement Engineering without duplicating its generic workflows. They read the target repository's instructions, source, tests, and maintained architecture guide. Usage remains the public sample and package-consumer surface; Adapter owns public product documentation and Core owns private implementation documentation. Additional Usage skills require a distinct, repeated workflow established by scoped instructions or maintained architecture documentation.

## Use locally

From this repository root, register the marketplace:

```powershell
codex plugin marketplace add .
codex plugin marketplace list
```

In the Codex app, open the plugin directory, select **TplQueue Agent Marketplace**, and install **Engineering**, **TplQueue Adapter**, **TplQueue Abstractions**, or **TplQueue Usage** as needed. Start a new thread to use the installed skills. For example, request `$review-csharp` for staged C# changes, `$review-adapter-architecture` for Adapter architecture, `$modify-abstractions-public-contracts` for an Abstractions contract change, or `$validate-usage-package-consumption` for Usage consumer validation.

This repository uses the marketplace identifier `tplqueue-agent-marketplace`; registration associates that identifier with this checkout. If another marketplace already uses that identifier, resolve the naming conflict before registering it.

## Maintain the package

Keep the portable and compatibility manifests' shared identity, version, description, author, keywords, and declared license synchronized. Validate plugin metadata and each changed skill before distribution. When refreshing an installed local plugin, use the plugin-creator update/reinstall workflow; if its cachebuster helper changes the compatibility manifest version, synchronize the portable version before reinstalling.

Local registration makes this checkout available to Codex. It does not publish the plugins to the public directory. The repository is covered by the [MIT license](LICENSE); no public repository URL is declared in the plugin manifests.

Package layout, marketplace conventions, and local registration follow the [official OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins).
