---
name: review-concurrency
description: Review C# changes involving multithreading, Tasks or async-await, concurrent collections, queues or schedulers, cancellation, retries, parallel execution, asynchronous observers or events, ordering guarantees, or shared mutable state. Provide focused concurrency analysis alongside normal C# review competence.
---

# Review concurrency

Analyze and report by default. Modify code only when the human explicitly requests fixes; keep them within the authorized scope. Assume professional C# knowledge and focus on concrete concurrency defects and risks.

## Establish the model

- Identify the requested diff or commit and follow applicable repository review prerequisites, including staged review when required. Do not stage changes automatically.
- Read applicable common and nearest scoped `AGENTS.md`, project configuration, and relevant architecture documentation. Retrieve concurrency invariants, compatibility targets, and identity/attempt semantics from those sources. Do not invent repository-specific rules. State missing or conflicting contracts as uncertainties.
- Before judging the code, map concurrent entry points, tasks, workers, schedulers, synchronization contexts, callbacks, and shared resources. Establish who owns work, completion, cancellation, errors, and shutdown. Distinguish documented guarantees from behavior merely observed in tests.

## Inspect concurrent paths

- Trace all state reachable from concurrent paths, including captured variables, referenced objects, static state, dependencies, and state accessed by observers. Verify safe publication, visibility, atomic transitions, and synchronization across all readers and writers. A concurrent collection does not make its values or compound operations automatically safe.
- Look for races, unsafe shared mutation, incorrect synchronization, inconsistent lock ordering, deadlock or livelock, and unsafely captured state. Consider reentrancy and external callbacks invoked while holding locks where relevant.
- Trace success, failure, cancellation, retry, and shutdown paths for double execution, missed completion, duplicate callbacks, and resources left in use. Check whether terminal transitions can compete or leave waiters unresolved.

## Check cancellation and retries

- Establish cancellation ownership and propagation. Check token-source and registration lifetimes, linked cancellation, cancellation during waits or retries, resource cleanup, repeated cancellation, and isolation from unrelated operations. Follow races between cancellation, completion, and disposal.
- Verify that retry does not unintentionally duplicate business effects, including when an attempt performs an effect before failing or losing its acknowledgement. Check consistency between attempts and whether prior work can overlap a later attempt.
- Require explicit cancellation/retry interaction: determine whether cancellation stops waits, prevents further attempts, and allows active work to settle safely. Preserve identity and attempt semantics declared by repository rules; do not assume particular semantics.

## Check ordering and async lifecycle

- Distinguish scheduling order, execution order, and observer/callback order. Never infer guarantees absent from the implementation or API contract. Trace how documented guarantees are enforced across parallelism, retries, failures, and cancellation.
- Verify that asynchronous work is awaited or has explicit lifecycle and error ownership. Check exception observation, unobserved Task failures, and fire-and-forget work that may outlive its owner. Apply `ConfigureAwait` expectations only where repository or platform rules make them relevant.
- Inspect async resource disposal, stop/drain behavior, and callbacks racing with stop or disposal. Check overlapping callback execution, event subscriptions, and whether callbacks can access disposed state or start work after shutdown.

## Assess tests and report

- Prefer controllable synchronization, barriers, completion signals, or controlled schedulers over timing sleeps. Require deterministic assertions where practical; stress runs alone do not establish correctness.
- Check coverage of race, cancellation, retry, completion, and error boundaries relevant to the changed behavior. Identify missing scenarios without generating tests unless fixes were requested.
- For each finding, give the affected file and line or symbol when available, the execution scenario or interleaving that exposes it, the violated contract, the consequence, and the safest next step. Distinguish demonstrated defects, conditional risks, and test gaps; do not manufacture findings.
- Prioritize by impact and follow repository reporting conventions. State the reviewed scope, validation performed, and material uncertainties. Report architectural problems without silently redesigning the code. If no actionable findings remain, say so explicitly.
