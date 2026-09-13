---
name: blind-first-pass
description: "Sets up a second opinion so it is worth having: the reviewing voice gets the reproduction, the raw numbers and the diff, but not your hypothesis, your discarded alternatives or the order you thought in. Use when asking another model or agent to review, when consulting a stronger model at a decision point, when two agents disagree and you are tempted to average them, when a review keeps agreeing with you, or when deciding whether a question wants a second opinion at all rather than a second measurement. Not for the mechanics of dispatching (use delegation-contract) and not for checking a single fact yourself (use measure-before-asserting)."
---

# The blind first pass

A second voice is worth something because it did **not** watch your solution take shape.
Telling it how you got there destroys precisely that. Most disappointing reviews are
disappointing for this reason and not because the reviewing model was weak.

## Staged disclosure

**Stage A — blind.** Reproduction, expected behaviour, raw measurements, the diff, fixed
decisions already made by a human, acceptance criteria.

Not: your hypothesis about the cause. Not the alternatives you discarded. Not the order in
which you thought about it. And note that the *selection* of facts anchors too — "we have
already looked at the cache" points at the cache just as firmly as saying you suspect it.

**Stage B — the experiment log, neutrally.** "With the cache disabled the fault still
occurred 17 times in 50 runs" — not "so it is not the cache."

**Stage C — comparing hypotheses.** Your reading, last, explicitly labelled as a hypothesis.

Stage A is overridden when repeating the work would be expensive or dangerous, when it
touches external systems or real data, or when leftovers from an attempt are sitting in the
working tree and would otherwise be mistaken for intended code.

## When a second voice adds information, and when it only adds agreement

**Closed, checkable questions** — is this function called, does the test pass, what does the
specification say — gain nothing from a second *opinion*. What they want is a second
independent **measurement**: "determine this from the primary source, the code path, or an
executable test." Convergence along separate evidence paths is a result. Bare agreement
with no evidence of its own is placebo.

**Open search spaces** genuinely gain: overlooked error paths, architectural assumptions
that break under load, missing security boundaries, invariants the tests do not actually
prove, dominant trade-offs, causes nobody investigated. Code review belongs here.

Pseudo-diversity is produced by:

- letting the second agent read the first agent's reasoning first,
- phrasing the task as "check whether this solution is good,"
- having several models vote on the same unevidenced claim,
- routing the same model through a different interface. A different pipe to the same weights
  is the same weights. It buys latency and cost, not another perspective.

**A better second question usually beats a third model.** Ask the second voice for the
strongest counter-hypothesis, one decisive test, and its own falsification criterion. A
third voice earns its place when two evidence-backed positions stay incompatible, the search
space is open and a mistake is expensive, no cheap decisive test exists, or the decision is
hard to reverse.

## Keep the roles apart

- The **blind explorer** does not see the other answers and searches independently.
- The **arbiter** receives both positions and checks their load-bearing claims — and is by
  that fact no longer independent.

One agent cannot be both. If you want an arbiter, dispatch a third.

The cheapest mechanical way to get a blind pass is context separation: dispatch a fresh
agent and give it the artefact, not the transcript. What makes it blind is the *context
boundary*, not the fact that it is a different model.

**Whoever picks the excerpt determines the verdict.** If the reviewer receives its slice
from the party being reviewed, it reviews that party's view of the problem — in a separate
context, but on the same slice. The checkable property is not "who reviews" but "who
selects." Where it matters, let the tool draw the sample, not the author.

## Put disagreement in front of the human — do not average it

Conflict between voices is the desired outcome. It is never suppressed and never stirred
into a mean. Break it into checkable claims so that only the real value or risk decision
reaches a person:

| Claim | Position A + evidence | Position B + evidence | Decisive test |
|---|---|---|---|

Then, in this order: run the deterministic test; failing that, take the smallest reversible
probe that produces information; failing that, compare error cost and reversibility; only
the remainder goes to the human.

Shared norm: **no agent owes another agreement. Both owe the human a falsifiable claim, the
best counter-evidence, and a visible revision trail.** Forced consensus destroys exactly the
diversity being paid for. "Unchanged" is a position, not a cooperation failure, when the new
evidence does not touch the claim.

You have merely been polite if your conclusion changes but no premise does; if you repeat
the other voice's wording without resolving your earlier evidence; or if your supposed
revision implies no different prediction.

## Placement, not frequency

Consulting a stronger model is worth its latency at decision points, not continuously:
before expensive or irreversible work, when you are stuck, and before a binding artefact is
finished. The consulting agent stays responsible for the outcome; the advice is material,
not an order.

The initial assignment of which voice is good at what is a starting value, not a law. Watch
who actually delivers. After two failed attempts change the level rather than resampling the
same agent — a retry is not a diagnostic instrument. Whoever retries should first name the
time-dependent cause they are assuming; if they cannot name one, looking is the right move.

## What actually enforces this

| Rule | Enforcement |
|---|---|
| The reviewer does not see your reasoning | **Enforced** by the context boundary, if you dispatch a fresh agent and hand it the artefact. It is not enforced by asking a model to ignore what it already read. |
| The reviewer's sample was not chosen by the author | **Enforceable** — have the tool draw it. Usually left as a behaviour rule. |
| Stages A/B/C in order | **Behaviour rule.** |
| Disagreement is surfaced rather than averaged | **Behaviour rule**, and the one most quietly broken, because a smoothed summary reads better than a table. |

A review that produces zero findings on a non-trivial artefact in its first round is itself
a finding: either the disclosure was not blind, or the task was phrased as a request for
approval.
