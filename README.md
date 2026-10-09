# TplQueue Agent Marketplace

A repository marketplace containing **TplQueue Overview**, with seven consumer integration and navigation workflows; **Engineering**, with portable C# and .NET engineering workflows; **TplQueue Adapter**, with seven instruction-only Adapter workflows; **TplQueue Abstractions**, with three instruction-only contract workflows; **TplQueue Usage**, with six instruction-only sample and consumer workflows; and **TplQueue Core Public**, with seven instruction-only public Core consumption workflows. The skills obtain authoritative architecture, compatibility targets, runtime invariants, and validation commands from current public contracts and the repository in which they are used.

**Start with [TplQueue Overview](plugins/tplqueue-overview/plugin.json)** when adopting TplQueue, choosing packages, understanding how Core and Adapter fit together, or integrating with application hosting technologies. It covers direct composition, Microsoft DI, ASP.NET Core/SignalR, legacy .NET, and DataJobs/cache/handlers. Deeper runtime, implementation, contract-change, and sample questions route to the owning plugins below; private Core source is not required.

## Package layout

- [Marketplace catalog](.agents/plugins/marketplace.json): the `tplqueue-agent-marketplace` marketplace, ordered as `tplqueue-overview`, `engineering`, `tplqueue-adapter`, `tplqueue-abstractions`, `tplqueue-usage`, and `tplqueue-core-public`.
- Portable manifests: [TplQueue Overview](plugins/tplqueue-overview/plugin.json), [Engineering](plugins/engineering/plugin.json), [TplQueue Adapter](plugins/tplqueue-adapter/plugin.json), [TplQueue Abstractions](plugins/tplqueue-abstractions/plugin.json), [TplQueue Usage](plugins/tplqueue-usage/plugin.json), and [TplQueue Core Public](plugins/tplqueue-core-public/plugin.json), each canonical for its plugin's identity and version.
- Codex compatibility manifests: [TplQueue Overview](plugins/tplqueue-overview/.codex-plugin/plugin.json), [Engineering](plugins/engineering/.codex-plugin/plugin.json), [TplQueue Adapter](plugins/tplqueue-adapter/.codex-plugin/plugin.json), [TplQueue Abstractions](plugins/tplqueue-abstractions/.codex-plugin/plugin.json), [TplQueue Usage](plugins/tplqueue-usage/.codex-plugin/plugin.json), and [TplQueue Core Public](plugins/tplqueue-core-public/.codex-plugin/plugin.json), with matching metadata, skill discovery, and OpenAI presentation.
- Skills: [seven Overview workflows](plugins/tplqueue-overview/skills/), [eleven Engineering workflows](plugins/engineering/skills/), [seven Adapter workflows](plugins/tplqueue-adapter/skills/), [three Abstractions workflows](plugins/tplqueue-abstractions/skills/), [six Usage workflows](plugins/tplqueue-usage/skills/), and [seven Core Public workflows](plugins/tplqueue-core-public/skills/).

Marketplace sources `./plugins/tplqueue-overview`, `./plugins/engineering`, `./plugins/tplqueue-adapter`, `./plugins/tplqueue-abstractions`, `./plugins/tplqueue-usage`, and `./plugins/tplqueue-core-public` resolve relative to this repository root. Skills live at each plugin root under `skills/`. OpenAI metadata is supplied by each compatibility manifest; the portable manifests intentionally have no `extensions.com.openai` override.

## Included skills

### TplQueue Overview

| Skill | Purpose |
| --- | --- |
| [understand-tplqueue-architecture](plugins/tplqueue-overview/skills/understand-tplqueue-architecture/SKILL.md) | Explain Core/Adapter composition, repository ownership, public entry points, and the separate Core license boundary. |
| [choose-tplqueue-packages](plugins/tplqueue-overview/skills/choose-tplqueue-packages/SKILL.md) | Choose a focused package set and connect integration modules and named retries to execution. |
| [integrate-tplqueue-application](plugins/tplqueue-overview/skills/integrate-tplqueue-application/SKILL.md) | Compose directly or through DI, select narrow interfaces, and assign shared queue ownership. |
| [integrate-tplqueue-dependency-injection](plugins/tplqueue-overview/skills/integrate-tplqueue-dependency-injection/SKILL.md) | Distinguish AddTplQueue registrations/snapshots from application-owned queues, caches, factories, and handlers. |
| [integrate-tplqueue-web-application](plugins/tplqueue-overview/skills/integrate-tplqueue-web-application/SKILL.md) | Integrate ASP.NET Core, hosted services, HTTP, and observer-driven SignalR DTO projection. |
| [integrate-tplqueue-legacy-application](plugins/tplqueue-overview/skills/integrate-tplqueue-legacy-application/SKILL.md) | Adopt incrementally in legacy .NET, validate compatibility, retain existing composition, and dispatch UI updates. |
| [compose-datajobs-cache-and-handlers](plugins/tplqueue-overview/skills/compose-datajobs-cache-and-handlers/SKILL.md) | Compose DataJobs, payload identities, HandlerKey registration, serializers, cache providers, and CacheQ. |

Overview owns application integration and navigation across packages. Core Public owns detailed runtime semantics; Adapter owns implementation/change workflows; Abstractions owns public contract ownership/change workflows; Usage owns samples and package-consumer validation; Engineering owns generic engineering workflows. Overview routes deeper questions to those owners without duplicating detailed Core observer, retry, CacheQ or lifecycle semantics, repository modification procedures, or sample-specific architecture.

The Overview plugin and its instructions are MIT; that license does not change Core package/source licensing. Core execution requires its applicable package/license terms, and private source access requires authorization under the applicable model. The final Core license type is not finalized in this integration baseline; no pricing, rights, tiers or support terms are inferred. Adapter's `docs/<lang>/` remains the product-documentation and public-site sync source, while Usage owns public sample documentation.

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

### TplQueue Core Public

| Skill | Purpose |
| --- | --- |
| [review-core-public-surface](plugins/tplqueue-core-public/skills/review-core-public-surface/SKILL.md) | Review public architecture, consumer semantics, and private-knowledge exposure. |
| [use-core-factory-facade](plugins/tplqueue-core-public/skills/use-core-factory-facade/SKILL.md) | Use the public factory facade and controlled construction boundary. |
| [use-core-queue-lifecycle](plugins/tplqueue-core-public/skills/use-core-queue-lifecycle/SKILL.md) | Interpret execution capacity, pause/resume, waiting, cancellation, and disposal. |
| [observe-core-job-lifecycle](plugins/tplqueue-core-public/skills/observe-core-job-lifecycle/SKILL.md) | Observe asynchronous lifecycle snapshots, failures, and execution channels. |
| [configure-core-retry-behavior](plugins/tplqueue-core-public/skills/configure-core-retry-behavior/SKILL.md) | Preserve root/queue policy precedence, explicit NoRetry, and failure semantics. |
| [use-core-cacheq-semantics](plugins/tplqueue-core-public/skills/use-core-cacheq-semantics/SKILL.md) | Distinguish queue completion, asynchronous cache acknowledgment, and disposal. |
| [use-core-datajob-payload](plugins/tplqueue-core-public/skills/use-core-datajob-payload/SKILL.md) | Use payload-aware factories, identities, handler routing, and supported rehydration. |

Core implementation remains private. The Core package is publicly consumable under its own package license; this Agent plugin and its instructions are MIT. The plugin license does not change or replace Core package/source licensing. This plugin contains only public, consumer-safe knowledge. Implementation-specific Core development guidance remains outside this public plugin; authorized private inspection may verify semantics but must not supply private implementation material for publication.

Core Public complements Overview's integration/navigation guidance, Engineering's generic procedures, Abstractions' contract-change workflows, Adapter's implementation and DI/configuration change workflows, and Usage's sample workflows. Public product documentation continues to be owned and published from Adapter's language trees. These skills guide agents using current public contracts, maintained public documentation, and observable behavior; public usage does not require a private Core checkout.

Add future Core Public skills only for distinct, repeated, independently triggerable consumer workflows or semantic models that are useful beyond one implementation file, stable enough for public guidance, and not already owned by another plugin. Do not create one skill per internal class. Maintain private implementation workflows separately.

## Use locally

From this repository root, register the marketplace:

```powershell
codex plugin marketplace add .
codex plugin marketplace list
```

In the Codex app, open the plugin directory, select **TplQueue Agent Marketplace**, and start with **TplQueue Overview** for consumer integration. Add **Engineering**, **TplQueue Adapter**, **TplQueue Abstractions**, **TplQueue Usage**, or **TplQueue Core Public** as needed. Start a new thread to use the installed skills. For example, request `$choose-tplqueue-packages` for adoption, `$integrate-tplqueue-dependency-injection` for application DI composition, `$review-csharp` for staged C# changes, `$review-adapter-architecture` for Adapter architecture, `$modify-abstractions-public-contracts` for an Abstractions contract change, `$validate-usage-package-consumption` for Usage consumer validation, or `$use-core-queue-lifecycle` for public Core queue guidance.

This repository uses the marketplace identifier `tplqueue-agent-marketplace`; registration associates that identifier with this checkout. If another marketplace already uses that identifier, resolve the naming conflict before registering it.

## Maintain the package

Keep the portable and compatibility manifests' shared identity, version, description, author, keywords, and declared license synchronized. Validate plugin metadata and each changed skill before distribution. When refreshing an installed local plugin, use the plugin-creator update/reinstall workflow; if its cachebuster helper changes the compatibility manifest version, synchronize the portable version before reinstalling.

Use skill-creator's `scripts/quick_validate.py` for each changed skill folder and plugin-creator's `scripts/validate_plugin.py` for the plugin root. Validate canonical `plugin.json` against its declared portable schema as well; the Codex validator checks the compatibility manifest. Check shared manifest metadata, marketplace source resolution, local Markdown links, and the final diff. For Overview and Core Public, inspect all new files for private source/test snippets, internal type descriptions, algorithms, synchronization/buffering/scheduler details, private paths, and implementation diagrams before distribution. Keep this README, [AGENTS.md](AGENTS.md), and catalog aligned when package boundaries change.

Local registration makes this checkout available to Codex. It does not publish the plugins to the public directory. The repository is covered by the [MIT license](LICENSE); no public repository URL is declared in the plugin manifests.

Package layout, marketplace conventions, and local registration follow the [official OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins).
