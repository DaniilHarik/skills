# Concurrency Review Reference

Use this reference when the target Go codebase relies on goroutines, channels, locks, atomics, background workers, or cancellation-sensitive I/O.

## Decision Order

Apply this order before endorsing a concurrent design:

1. Remove concurrency entirely if latency, throughput, or responsiveness does not require it.
2. Prefer a single owner goroutine or plain sequential code when only one component needs to mutate state.
3. Prefer `errgroup.WithContext` for parallel sibling tasks tied to a request or lifecycle.
4. Prefer `sync.Mutex` for protecting in-memory shared state.
5. Prefer channels when the design is fundamentally about handoff, coordination, or bounded work queues.
6. Prefer atomics only for isolated counters, flags, or pointers with trivial invariants.

If the implementation skips to a more complex option without evidence, flag it.

## Pattern Selection

### No concurrency

Prefer this when:

- Work is short-lived or I/O is already serialized by a dependency.
- The concurrent form adds bookkeeping but no measurable benefit.
- Correctness depends on strict ordering and the sequential form is straightforward.

### `errgroup.WithContext`

Prefer this when:

- A parent operation starts a few sibling goroutines and wants first-error cancellation.
- Results are joined before returning.
- Each goroutine should stop when the shared context is canceled.

Flag:

- Hand-rolled error channels plus `WaitGroup` when `errgroup` would be clearer.
- Goroutines that ignore the derived context.

### `sync.Mutex`

Prefer this when:

- Multiple goroutines share mutable state in memory.
- Invariants span multiple fields.
- Reads and writes both need a coherent snapshot.

Flag:

- Channel-based serialization for plain shared state when a mutex would be simpler.
- Atomics spread across related fields.
- Lock hold times inflated by logging, blocking I/O, or callbacks.

### `sync.RWMutex`

Require evidence before endorsing it.

Flag when:

- Write frequency is non-trivial.
- Protected operations are cheap.
- The code adds read/write lock complexity without benchmark evidence.

### Channels

Prefer this when:

- Ownership of data moves from producer to consumer.
- A pipeline or worker queue is the core abstraction.
- Backpressure or bounded buffering is part of the design.

Flag:

- Channels used only to avoid a mutex.
- Multiple goroutines closing the same channel.
- Buffered channel sizes chosen arbitrarily.
- Sends or receives that can block forever after cancellation.
- `default` cases that turn coordination into a spin loop.

Review questions:

- Who owns channel close responsibility?
- What prevents producers from blocking forever?
- What drains results on early return?
- Is buffering part of a requirement or just a guess?

### Worker pools

Require evidence before endorsing them.

Prefer:

- A fixed worker pool only when work is plentiful, homogeneous, and parallelizable.
- Direct goroutine spawning only when cardinality is naturally small and bounded.

Flag:

- Pools added for tiny workloads or single-request code paths.
- Pools without queue bounds, cancellation, or failure propagation.
- Nested concurrency inside workers that defeats the intended bound.

### Atomics

Prefer this only when:

- One variable changes independently.
- The team can explain the memory ordering assumptions.
- The atomic form is materially simpler or faster than a mutex.

Flag:

- Atomics used as a style preference.
- Compare-and-swap loops around multi-step invariants.
- Mixtures of atomic and non-atomic access to the same field.

## Lifecycle Hazards

Always check for:

- Goroutines launched in loops without bounded lifetime.
- Missing `defer wg.Done()` after successful `Add`.
- `WaitGroup.Add` racing with goroutine start or `Wait`.
- Timers and tickers not stopped when owners exit.
- Background goroutines tied to request-scoped objects.
- Result channels that are never closed or are closed by the wrong side.
- Cancellation that stops receives but not sends, or vice versa.

## Efficiency Rules

- Do not call a concurrent design efficient unless it is also simpler than the alternatives or backed by workload evidence.
- Prefer fewer goroutines, fewer queues, and fewer synchronization points.
- Avoid buffering, pooling, or lock splitting until contention or throughput is demonstrated.
- Avoid copying large values across channels when pointers or indices preserve ownership safely.
- Prefer measuring with benchmarks or profiles over assuming concurrency improves performance.

## Review Language

When reporting issues:

- State whether the problem is correctness, leak risk, race risk, complexity, or unproven optimization.
- Recommend the simplest replacement pattern, not just a generic cleanup.
- Say explicitly when a more complex concurrent design may still be valid if benchmark or workload evidence exists.
