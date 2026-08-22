---
name: create-mental-model
description: Use this skill when the user wants a mental model, intuition, a framework, or a way to "actually understand" or "think about" a topic rather than a summary of it. Triggers include "give me a mental model for X", "how should I think about X", "build me intuition for X", "explain the relationships between these terms", "I keep getting confused by X", or a request to make sense of a set of jargon that overlaps confusingly. Also use when the user has just received an explanation or diagram and asks for something they can carry around and reuse. Do NOT use for straightforward factual lookups, step-by-step how-tos, or when the user needs the actual detail rather than a compression of it.
disable-model-invocation: true
---

# Building mental models

A mental model is not a summary. A summary lists what is true; a model is a **compression that regenerates what is true** and lets the user answer questions they haven't been asked yet.

The test: if the user forgets everything except one sentence, can they still reason correctly about the domain? If no, you wrote a summary.

## Before building: get the material right

Compressing a domain you have wrong produces a confidently wrong model, which is worse than no model. So:

- If the topic involves current facts, live figures, named entities, or anything that may have moved, **search first**. Do not model from memory.
- Write out the actual terms, entities, and relationships in plain form before attempting any compression. You cannot compress what you haven't laid out.
- If the user supplied the term list, treat that list as the scope boundary. This is common when they paste jargon. Every term they named must land somewhere in the model.

## Step 1: Name the category error

**Every good model is built against a specific confusion.** Find it before anything else.

Ask: what mistake does this domain reliably produce in competent people? Usually it's that two terms live on different ontological levels but get discussed as peers. Common level-confusions worth checking for:

| Confusion | Example |
|---|---|
| Thing vs. a measurement of it | A person vs. their height in centimetres |
| Stock vs. flow | Water in a bath vs. water arriving from the tap each minute |
| Cause vs. marker | Wet pavement can indicate rain without causing the rain |
| Estimate vs. quantity | The predicted journey time vs. the minutes the journey takes |
| Category vs. state | An overdue library book is in a state, not a different type of book |
| Capability vs. artifact | A musician's ability to play vs. the recording they made |
| Constraint vs. goal | A spending limit vs. the goal of a restorative holiday |

If you can't name the confusion, you don't understand the domain well enough to compress it yet. Go back and read more.

## Step 2: Source the analogy from the domain's own mechanics

The analogy must be **structural, not decorative**. It works when the domain genuinely shares a shape with the source. In software delivery, a deployment pipeline is a useful model because changes really do move through ordered gates toward production. That model keeps producing correct answers beyond the first paragraph.

Validation test: push the analogy into three of the domain's *hard* cases. If it survives all three, keep it. If it survives only the easy cases, discard it and find another. A metaphor that breaks exactly where the interesting questions live is worse than plain language.

Rules:
- **One analogy per model.** Two competing metaphors cancel out.
- Prefer a source domain the *specific user* already has fluency in. Use what you know about them.
- Never reach for a stock metaphor ("it's like a library / a highway / an orchestra") without first testing whether it's load-bearing. It usually isn't.
- Plain language beats a weak analogy. If nothing structural fits, build the model on the governing rule alone and skip Step 2.

## Step 3: Compress to a generative rule

One sentence that regenerates the rest. This is the actual deliverable; everything else is scaffolding around it.

> Incident impact ≈ affected traffic × time to recovery.

Good generative rules are *causal and quantitative in shape*. They name what drives what, and in what proportion. Bad ones are definitional restatements ("code quality matters") or hedged mush.

Sanity check: does the rule imply at least two non-obvious consequences? If it only implies things the user already believed, compress harder.

## Step 4: Give a procedure

Models nobody can *run* don't get used. Provide an ordered sequence: what to look at first, second, third, and what to do with each. Ordering carries information. Put the highest-leverage input first and say why the conventional first input isn't it, if that's true.

## Step 5: Test the model in public

Take four to six questions that genuinely confuse people in this domain and show the model resolving each in one or two lines. This does double duty: it validates the model and it's the section that persuades the reader to adopt it.

Choose real confusions, not softballs. If the model can only handle questions you invented for it, it isn't ready.

## Step 6: Mark the failure boundary

**Mandatory.** State where the model misleads. An unbounded model gets over-driven. The user will push it into territory it doesn't cover and reach a confident wrong conclusion, then blame the domain rather than the model.

Include the places the analogy's internal logic predicts something false, and any domain where the user should stop reasoning by model and consult the actual detail (dosing, compliance, legal thresholds, anything with a real downside).

## Output shape

Deliver in this order, and keep it to roughly one page plus the test cases:

1. **The model.** Name the analogy and include a compact term-to-role mapping. A table works well when there are more than four terms.
2. **The governing rule.** Set it off visually. It's the payload.
3. **How to apply it.** Give the numbered procedure.
4. **Where it earns its keep.** Include the test cases.
5. **The one-line version.** Give the retention version.
6. **Where it breaks down**

Save as a markdown file when the user will reuse it; answer inline when it's part of a live conversation.

## Anti-patterns

- **The glossary in costume.** Terms with cute labels attached and no generative rule. Check: delete the mapping table. Is there still a model? There should be.
- **Analogy that only covers the easy cases.** See Step 2 validation.
- **The consensus summary with a metaphor stapled on.** If the model reorganises nothing and overturns no intuition, it isn't compressing. It's decorating.
- **Precision theatre.** Invented thresholds, fake numbers, and false specificity to signal rigour. Say "roughly proportional" if that's what you mean.
- **Length as substitute for compression.** A model that takes four pages to state has failed at its only job.
- **Skipping Step 6** because the model feels elegant. Elegant models are the dangerous ones.
