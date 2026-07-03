---
name: climb-abstraction-ladder
description: Help software engineering leaders climb and descend the Ladder of Abstraction between concrete engineering signals, operational mechanisms, and strategic choices. Use when the user provides a concrete signal, evidence, ticket, incident, metric, architecture problem, delivery problem, or leadership concern and wants to climb from concrete to operational to strategic, descend from strategic intent back to operational changes and concrete proof, avoid vague strategy language, or turn engineering observations into leadership-ready framing.
---

# Climb Abstraction Ladder

## Overview

Use this skill to translate engineering reality into leadership judgment without losing the evidence. Move one rung at a time: concrete signal -> operational mechanism -> strategic choice, then back down into operational intervention -> concrete proof.

Read [references/engineering-ladder.md](./references/engineering-ladder.md) when the user asks for examples, wants a reusable template, provides a messy or ambiguous signal, or needs coaching language for an engineering leadership conversation.

## Core Model

Use exactly three rungs unless the user explicitly asks for a different model:

- Concrete signal: Observable evidence such as an incident, queue, metric, ticket, customer complaint, code symptom, latency spike, flaky test, blocked review, or repeated escalation.
- Operational mechanism: The repeatable system, handoff, ownership rule, design choice, process, team topology, architecture constraint, or delivery flow that keeps producing the signal.
- Strategic choice: The leadership trade-off or decision that affects business, customer, roadmap, reliability, cost, speed, retention, or organizational leverage.

Treat "operational" and "operating pattern" as the same middle layer. Prefer "operational mechanism" in outputs unless the user uses a different label.

## Workflow

1. Separate facts from interpretation.
   Name what is directly known, what is inferred, and what is missing. If the user gives only a vague concern, make the smallest reasonable assumption and say it is an assumption.
2. Identify the starting rung.
   If the input is an observable fact, start at concrete. If it names a process, ownership, architecture, or delivery pattern, start at operational. If it names a goal, bet, policy, investment, or business consequence, start at strategic.
3. Move only one rung per step.
   Do not jump from a concrete signal directly to strategy. Name the operational mechanism first.
4. Preserve the evidence chain.
   Every operational claim must point back to concrete evidence. Every strategic claim must point back to the operational mechanism.
5. End with a usable next move.
   Provide either a better question, a leadership framing sentence, a decision option, or the first concrete signal to inspect.

## Climbing Up

For concrete -> operational:

- Ask: "What repeatable mechanism could make this keep happening?"
- Look for ownership boundaries, dependency handoffs, release flow, review capacity, platform constraints, architecture coupling, observability gaps, unclear decision rights, or incentive mismatches.
- Output a mechanism, not a complaint. Prefer "schema review ownership is a release bottleneck" over "reviews are slow."

For operational -> strategic:

- Ask: "If this mechanism persists, what important outcome degrades?"
- Tie the mechanism to a strategic stake: launch readiness, reliability promise, customer retention, cost-to-serve, time-to-market, support burden, engineering capacity, or product scope.
- Turn the stake into a choice: change ownership, fund a platform bet, narrow scope, change sequencing, set a policy, revise a reliability target, or accept the cost deliberately.

## Descending Down

For strategic -> operational:

- Ask: "What operating change would make this strategy real?"
- Convert themes into controllable levers: ownership boundary, staffing investment, review cadence, platform policy, escalation rule, architecture constraint, roadmap sequencing, service-level target, or decision forum.

For operational -> concrete:

- Ask: "What would change first if this operational intervention worked?"
- Name observable proof: a metric, queue length, p95/p99 latency, incident class, retry rate, lead time, review wait, deployment frequency, escaped defects, customer ticket volume, on-call pages, or decision turnaround time.
- Prefer signals visible within days or weeks over lagging annual outcomes.

## Output Shapes

Choose the smallest useful shape for the user's request.

Use a two-step climb for a concrete signal:

```text
Concrete signal: ...
One level up, operational mechanism: ...
One more level up, strategic choice: ...
What to check next: ...
```

Use a round trip when the user wants strategy back to evidence:

```text
Strategic intent: ...
Operational intervention: ...
Concrete proof: ...
Decision boundary: ...
```

Use leadership phrasing when the user needs to speak in a meeting:

```text
Instead of: ...
Say: ...
Because: ...
```

## Guardrails

- Do not inflate every engineering pain into a strategic issue. If the strategic consequence is weak, say it is operationally important but not yet strategic.
- Do not use executive abstractions such as "alignment," "transformation," "platform maturity," or "operating model" unless they are anchored to a mechanism and proof.
- Do not recommend a reorg, rewrite, or large platform investment from one signal. Frame it as a hypothesis and name the evidence needed.
- Keep the user's original signal visible in the answer. The output should be traceable back to the concrete fact.
- If the user asks for coaching, be direct and practical rather than inspirational.
