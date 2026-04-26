---
name: go-code-review
description: Review entire Go codebases for idiomatic Go, correctness, long-term maintainability, and concurrency design. Use when Codex is asked to review Go or Golang code, assess architecture and code quality, find behavioral or maintainability risks, evaluate goroutine/channel/mutex/context usage, or judge whether concurrent code can be simplified while preserving performance.
---

# Go Code Review

Review Go codebases with emphasis on idiomatic design, simple control flow, long-term maintainability, and concurrency that is justified, correct, and minimal.

## Review Workflow

1. Establish context before judging style.
   Read `go.mod`, entrypoints, touched packages, and nearby tests.
   Search for concurrency primitives with `rg -n 'go\\s|chan|select\\s*\\{|WaitGroup|Mutex|RWMutex|atomic\\.|context\\.|errgroup|Once|Pool|Ticker|Timer'`.
   Map package responsibilities, dependency direction, and where important state or orchestration logic lives.
2. Determine the intended behavior.
   Infer the contract of each package or function from callers, tests, comments, and exported API shape.
   Do not flag a pattern as non-idiomatic until its constraints are clear.
3. Review for correctness first, then maintainability, then simplicity and style.
   Prioritize behavioral bugs, deadlocks, leaks, races, cancellation gaps, broken error handling, and structural choices that will make future changes risky or expensive.
4. Treat concurrency as suspect until it earns its keep.
   Ask whether the code needs parallelism, ownership transfer, or coordination at all.
   Prefer removing concurrency when a sequential design is easier to reason about and fast enough.
5. Produce findings, not a rewrite dump.
   Report concrete issues with file and line references, explain impact, and suggest the simpler or more idiomatic pattern.

## Idiomatic Go Checks

- Prefer clear package boundaries and narrow exported APIs.
- Prefer small functions with direct control flow over abstraction layers that hide basic logic.
- Prefer explicit error handling with useful context over sentinel booleans or opaque wrapping.
- Prefer zero-value-friendly types, constructors only when invariants matter, and methods that preserve obvious ownership.
- Prefer simple slice, map, and pointer usage over defensive copying or pointer indirection without a measured need.
- Prefer interfaces at consumption boundaries, not premature interface definitions near implementations.
- Prefer naming that reflects behavior instead of type mechanics.
- Prefer standard library facilities before custom helpers when they express the intent well.

## Maintainability Checks

- Review the whole codebase, not just isolated diffs, when the task asks for a broader review.
- Flag packages that mix unrelated responsibilities or hide orchestration, I/O, state mutation, and domain rules in the same layer.
- Flag dependency direction that makes reuse, testing, or refactoring harder.
- Flag APIs that expose internal details, require callers to preserve hidden invariants, or make ownership unclear.
- Flag duplicated logic, near-duplicate workflows, and hand-rolled frameworks that increase change surface.
- Flag concurrency patterns that spread lifecycle control across packages instead of keeping ownership obvious.
- Flag error-handling patterns that make failures hard to trace or recovery behavior inconsistent.
- Flag tests that are too coupled to implementation details, leave important concurrency or integration paths unexercised, or make safe refactors harder.
- Prefer designs that make the next change obvious to a new maintainer without extensive tribal knowledge.

## Concurrency Review Rules

- Verify every goroutine has a clear start condition, owner, cancellation path, and exit condition.
- Check whether `context.Context` is propagated to blocking work and whether cancellation actually stops spawned goroutines.
- Prefer `errgroup.WithContext` for sibling tasks that should fail fast together.
- Prefer `sync.Mutex` for protecting shared mutable state.
- Prefer channels for ownership transfer, work distribution, or explicit coordination; do not treat channels as a default replacement for a mutex.
- Prefer atomics only for single independent values with simple invariants. Escalate to a mutex when multiple fields must move together.
- Be skeptical of `sync.RWMutex`, buffered channels, worker pools, and fan-out/fan-in topologies unless the code or workload clearly benefits.
- Check for goroutine leaks, blocked sends, blocked receives, unbounded queues, forgotten `Done` calls, missing `Stop` on timers or tickers, and error paths that skip cleanup.
- Check that select loops handle shutdown first and do not spin on `default` unless busy waiting is intentional and justified.

Read [references/concurrency-review.md](./references/concurrency-review.md) when the code uses non-trivial channels, mutexes, atomics, worker pools, or cancellation logic.

## Go Version Awareness

Read the target repo's `go.mod` before making version-sensitive comments.

- For Go versions before 1.26, actively look for places where Go 1.26 language, runtime, toolchain, or standard-library features could simplify the code, remove custom scaffolding, or reduce concurrency risk. Call these out as upgrade opportunities, not defects against the repo's current version.
- For Go versions before 1.22, check loop variable capture in goroutines and closures carefully.
- For Go 1.22 and later, do not raise the old loop-variable warning unless the code still takes addresses or otherwise shares mutable iteration state incorrectly.
- Prefer advice that matches the standard library and language features available in the repo's declared Go version.

## Review Output

Lead with findings ordered by severity.

For each finding:

- Name the issue precisely.
- Cite file and line.
- Explain the concrete failure mode, maintainability cost, or unnecessary complexity.
- State the simpler or more idiomatic alternative.
- Distinguish proven bugs from inference when evidence is incomplete.

When the issue is primarily maintainability, explain why the current structure will slow future changes, increase regression risk, or obscure ownership.

If no findings are discovered, say so explicitly and note residual risk areas such as missing race tests, absent benchmarks, or architecture areas that remain hard to evolve safely.
