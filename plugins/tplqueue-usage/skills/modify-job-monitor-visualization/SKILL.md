---
name: modify-job-monitor-visualization
description: Modify TplQueue.Usage's reusable JavaScript JobMonitor Web Component, including time/channel placement, enqueue history, collision handling, accessibility, and renderer boundaries without changing represented backend facts.
---

# Modify JobMonitor visualization

Complement engineering's `implement-feature` and, where C# integration changes, `testing-dotnet`, when available and relevant. Use [validate-job-monitor-interaction](../validate-job-monitor-interaction/SKILL.md) for interactive validation and [modify-blazor-monitor-projection](../modify-blazor-monitor-projection/SKILL.md) for affected projection/bridge behavior. Leave generic implementation and testing procedures to engineering.

## Establish the current frontend

Read applicable parent instructions, the target root `AGENTS.md` or `Agents.md` and delegated/scoped instructions, the maintained Blazor architecture guide, current JobMonitor documentation, and affected tests. Inspect the Web Component and Razor/JS snapshot bridge before changing rendering.

The active frontend is `tools/TplQueue.JobMonitor`, a reusable JavaScript Web Component connected through a small Razor/JS snapshot bridge. ScatterChart and vis-timeline are retired; do not reintroduce them because legacy test/support code remains.

The target repository and maintained architecture guide remain authoritative. Surface conflicts before dependent edits and preserve these semantics unless explicitly changed.

## Preserve time and channel meaning

- Time is vertical. Logical execution channels come from the backend; root identity never determines channel placement. Null/unassigned events use explicit Unassigned placement.
- The overview represents five seconds ending at the bottom reference. Preserve exact event timestamps and the fixed left UTC ruler.
- Handle collisions with count/group markers and temporary interval inspection according to maintained UI behavior. Never displace timestamps to make points fit.
- Time zoom must not widen execution channels. Preserve keyboard accessibility for grouped jobs.

## Preserve enqueue and execution positions

Assigned execution positions require the backend channel-bearing `Started` timestamp. Keep jobs without the channel/start pair in Unassigned; do not substitute `Running` or terminal timestamps for channel acquisition.

Preserve recorded enqueue time separately in typed `enqueuedAt` and metadata. Retain its marker in the collapsible Unassigned strip after assignment. When both positions are visible, draw the directed enqueue-to-start connector.

These are two observed positions of the same job, not additional jobs or dependency relationships. Search must reveal the selected job's applicable strip, including retained enqueue history. Do not invent missing enqueue observations.

## Preserve model and renderer boundaries

Keep real dependency edges separate from graph membership metadata. Keep model validation, graph traversal, layout, time control, and theme tokens outside the mechanical SVG renderer. The renderer renders an already interpreted model; it does not own domain semantics.

MSBuild synchronizes the maintained tool assets into an ignored sample `wwwroot` location before static-asset discovery. Edit source assets in the tool, never generated/synchronized copies.

The future TypeScript/OpenAPI architecture is a separate sample profile, not a required migration for this Web Component. `QueueObserverSignalRDashboard` is a distinct transport sample; do not attribute its custom hub or endpoints to the JobMonitor Blazor sample.

## Report

State the visualization contract changed, time/channel behavior, enqueue/start representation, collision behavior, accessibility impact, graph/dependency impact, renderer/model-boundary impact, generated-asset impact, and tests and validation.
