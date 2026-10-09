---
name: use-core-cacheq-semantics
description: Explain or review public CacheQ construction and payload-aware consumption, asynchronous terminal cache transitions, observer latency, WaitAsync and disposal limits, and unsupported durability or leasing-timing assumptions.
---

# Use Core CacheQ semantics

Complement [queue lifecycle](../use-core-queue-lifecycle/SKILL.md), [Job observation](../observe-core-job-lifecycle/SKILL.md), [retry behavior](../configure-core-retry-behavior/SKILL.md), and engineering's `review-concurrency` when available and relevant.

Read applicable repository instructions, public CacheQ/cache/factory contracts, maintained public documentation, and relevant observable behavior. The target version is authoritative; flag contradictions before dependent changes. Private source may verify public semantics when access is authorized, but never publish private persistence, scheduling, synchronization, leasing algorithms, tracking structures, or implementation types.

## Construct and consume CacheQ

Describe CacheQ as a cache-backed queue integration for payload-aware DataJobs. It combines public queue execution, `IDataJobCache` integration, payload-aware Job graphs, and asynchronous terminal cache transitions.

Use the public queue factory's `IQFactory.CacheQ` with a cache creation capability (`Func<IDataJobCache>`), logger (`ILogger<ICacheQ>`), and supplied parallel dispatcher (`IParallelQ`), returning `ICacheQ`. Do not document internal adapters or infer dependency lifetime/disposal ownership beyond public contracts.

Use the cache-backed enqueue overloads for payload-aware DataJob roots supported by current contracts. Do not infer that every inherited enqueue overload persists arbitrary Jobs. Apply [DataJob/Payload](../use-core-datajob-payload/SKILL.md) for graph and payload assumptions.

## Separate completion from acknowledgment

Terminal cache transitions occur asynchronously from queue execution through the observation path. Therefore `WaitAsync` completion is not cache acknowledgment completion and does not prove that terminal cache state or persistence has been fully applied. Underlying dispatcher completion alone does not establish completion of all cache-backed work.

Slow observer delivery can delay CacheQ acknowledgment. This is downstream asynchronous latency; the observer does not execute inside the Job body. Use the observation skill's callback-performance guidance without exposing private delivery machinery.

CacheQ disposal is not an acknowledgment barrier. Pending observer-driven terminal transitions may remain unapplied; do not assume disposal waits for them. If durability or acknowledgment is required, identify the application's stronger requirement and a contract-supported way to verify it separately. Do not invent that guarantee from `WaitAsync`, terminal event arrival, or `Dispose`.

## Respect the current contract

Apply current public queue pause/resume semantics. Do not infer periodic cache leasing or a scheduling cadence from the existence of `LeasingPulseMs`. Such a claim requires support from current source, tests, and maintained public documentation; if they disagree, report the uncertainty. Update the guidance if the target contract deliberately stabilizes this behavior later.

Concrete `IDataJobCache` implementations belong outside the private Core kernel, normally in Adapter integrations. Route their configuration/development to `tplqueue-adapter` when available. Keep private cache internals out of this skill and public output.

## Report

State queue/cache role, DataJob requirement, underlying queue completion state, independently established cache acknowledgment state, observer-latency and disposal implications, and durability assumptions that remain unguaranteed.
