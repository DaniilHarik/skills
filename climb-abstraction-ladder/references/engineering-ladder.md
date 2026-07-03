# Engineering Ladder Reference

Use this reference for examples, templates, and calibration while applying the three-rung ladder in software engineering leadership contexts.

## Level Definitions

| Level | Belongs here | Useful question | Failure mode |
|---|---|---|---|
| Concrete signal | Error spikes, p95 latency, flaky tests, blocked tickets, customer complaints, on-call pages, review queues | What exactly happened, where, and how often | Arguing from anecdotes |
| Operational mechanism | Service boundaries, deployment flow, ownership model, architecture trade-offs, review bottlenecks, incident process, technical debt tax | What system, handoff, design choice, or ownership rule keeps producing this | Treating symptoms as isolated |
| Strategic choice | Time-to-market, customer retention, cost-to-serve, launch readiness, reliability posture, product scope, capacity allocation | What outcome degrades if this mechanism persists | Using executive language with no anchor |

## Move Patterns

Concrete -> operational:

- "Is this a one-off defect or a repeatable class of failure?"
- "What queue, handoff, boundary, or ownership rule is making this keep happening?"
- "What architecture or delivery trade-off explains why the local symptom keeps returning?"

Operational -> strategic:

- "If we leave this alone for two quarters, what customer, roadmap, reliability, or cost outcome degrades?"
- "Does fixing this remove a real bottleneck, or mostly reduce internal frustration?"
- "What decision are we avoiding: ownership, staffing, sequencing, policy, architecture, or scope?"

Strategic -> operational:

- "What operating change would make this strategic intent true?"
- "Which lever changes: ownership boundary, staffing investment, review cadence, platform policy, escalation rule, sequencing, or reliability target?"

Operational -> concrete:

- "What metric, queue, incident class, or delivery stage should change first?"
- "What would we inspect within days or weeks to know whether the intervention is working?"

## Examples

Latency complaint:

- Concrete signal: Checkout p95 latency rose from 250 ms to 900 ms during peak traffic.
- Operational mechanism: Checkout fans out to too many downstream services, and cache invalidation creates noisy misses during traffic spikes.
- Strategic choice: Peak conversion and infrastructure cost are at risk; choose whether to fund a checkout reliability push before the next launch.
- Operational intervention: Collapse two downstream calls and narrow cache invalidation scope.
- Concrete proof: Watch p95 latency, cache hit rate, checkout abandonment, and downstream error rate this week.

Review bottleneck:

- Concrete signal: Schema changes wait three to four days for approval.
- Operational mechanism: Database ownership is centralized with one overloaded reviewer, so cross-team work queues behind a single decision point.
- Strategic choice: Roadmap speed is constrained by data-platform decision rights; choose between delegated schema ownership, more reviewer capacity, or narrower product sequencing.
- Operational intervention: Define service-owned schema zones, required consultation cases, and an escalation path for blocked changes.
- Concrete proof: Track schema review wait time, blocked tickets, rework rate, and escalation count.

Recurring incidents:

- Concrete signal: The same alert fires every week after deployments.
- Operational mechanism: The service lacks fault isolation and the team has no clear owner for deployment health after handoff.
- Strategic choice: Reliability may stop being a product advantage; choose whether to change ownership and invest in deployment safety before adding more scope.
- Operational intervention: Assign service ownership, add deployment checks, and define rollback authority.
- Concrete proof: Track incident recurrence, rollback time, deploy failure rate, and after-hours pages.

Rewrite pressure:

- Concrete signal: Engineers say the codebase is too painful to work in.
- Operational mechanism: This is not yet enough. Look for concrete evidence: lead time, defect clusters, coupling hotspots, deploy risk, onboarding time, or repeated incident classes.
- Strategic choice: A rewrite is strategic only if it changes speed, stability, cost, or product optionality enough to beat incremental repair.
- Operational intervention: Run a bounded architecture diagnosis before proposing a platform bet.
- Concrete proof: Map hotspots, cycle time by area, change failure rate, and support cost.

## Meeting Language

Use this shape when turning an engineering observation into leadership language:

```text
The concrete signal is [observable evidence].
The operational mechanism appears to be [repeatable cause].
The strategic question is whether [important outcome] is now constrained enough to justify [decision lever].
The next proof point is [metric or observable check].
```

Example:

```text
The concrete signal is that schema changes are waiting three to four days for approval.
The operational mechanism appears to be centralized database decision rights with one overloaded reviewer.
The strategic question is whether roadmap speed is now constrained enough to justify delegated schema ownership or additional platform capacity.
The next proof point is review wait time and blocked-ticket count over the next two weeks.
```

## Calibration Rules

- If the user provides one isolated fact, produce a hypothesis and state what evidence would prove repetition.
- If the user provides repeated evidence, name the mechanism more confidently.
- If there is no customer, business, reliability, roadmap, cost, or capacity consequence, stop at operational.
- If the strategic claim cannot be walked back down to a concrete signal, it is too vague.
- If the proposed intervention cannot name an owner, cadence, metric, or decision boundary, it is not operational enough.
