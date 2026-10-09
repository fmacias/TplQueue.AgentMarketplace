Purpose of this repository
Public-only boundary
Portable-first policy
Where plugins live
Where Skills live
Do not introduce TplQueue private implementation knowledge
Keep root plugin.json canonical
Keep compatibility metadata synchronized
How to validate Skills/plugins

## Overview plugin boundary

`plugins/tplqueue-overview/` is the recommended consumer-facing front door for TplQueue adoption, package selection, architecture, and application integration across Core, Adapter, Abstractions, and Usage. Its seven portable, instruction-only skills cover application composition, Microsoft DI, ASP.NET/SignalR, legacy .NET, and DataJob/cache/handler wiring.

- Overview owns cross-repository application integration and navigation. Core Public owns detailed public runtime semantics; Adapter owns implementation/change workflows; Abstractions owns public contract ownership/change workflows; Usage owns samples and package-consumer validation; Engineering owns generic procedures.
- Follow current public contracts, package metadata/READMEs, maintained public documentation, and Usage evidence over an older skill or README. Surface contradictions; do not silently normalize other repositories or expand the authorized edit scope.
- Public guidance must not require private Core source or contain private implementation knowledge. Keep detailed Core retry, observer, CacheQ, and queue lifecycle semantics in Core Public, and route repository-specific modification procedures to their owners.
- This plugin is MIT independently of Core. Core execution and authorized private source access remain subject to their applicable terms; the final Core license type is not finalized in this baseline. Do not invent licensing rights or commercial terms.
- Adapter's `docs/<lang>/` remains the public product-documentation and site-sync source; Usage owns sample documentation. Overview does not create a new publishing source or change sync instructions.

## Core Public plugin boundary

`plugins/tplqueue-core-public/` contains seven portable, instruction-only skills for public Core package usage and observable runtime semantics. Current public contracts, maintained public documentation, and observable behavior remain authoritative over the packaged baseline. Surface contradictions before dependent edits.

- Keep Core implementation private. Authorized private inspection may verify public behavior, but do not copy private source/tests, algorithms, lock structure, internal collections/buffering, scheduler structure, unnecessary private type names, private paths, or implementation diagrams into this marketplace.
- Public use must not require private source access. Implementation-specific Core development guidance belongs outside this public plugin.
- The plugin and its instructions are MIT. Core package consumption remains governed by its own package license; marketplace licensing does not change Core package/source licensing.
- Overview owns application integration/navigation; Engineering owns generic procedures; Abstractions owns contract changes; Adapter owns concrete integration implementation, DI/named policy changes, and public product documentation; Usage owns samples and package-consumer validation. Keep these workflows distinct.
- Adapter's language trees remain the source for public product-documentation publishing. Agent guidance in this marketplace does not move that publishing boundary.
- New Core Public skills require distinct, repeated, independently triggerable consumer workflows or semantic models, useful beyond one implementation file, stable enough for public use, and not already owned elsewhere. Do not create one skill per internal Core class.

## Validation and distribution

Keep root `plugin.json` canonical and synchronize shared metadata with `.codex-plugin/plugin.json`. Follow the existing local catalog pattern in `.agents/plugins/marketplace.json`; keep README skill listings and repository-boundary wording aligned.

Before distribution, run skill-creator's `scripts/quick_validate.py` for every changed skill and plugin-creator's `scripts/validate_plugin.py` for the plugin root. Validate the portable manifest against its declared schema separately. Check shared metadata, catalog paths, local Markdown links, and the final diff, including untracked plugin files. Inspect Overview and Core Public content for private implementation knowledge as well as unsupported public guarantees; schema validation alone cannot establish those boundaries. Report unavailable validation explicitly.
