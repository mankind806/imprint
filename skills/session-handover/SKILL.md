---
name: session-handover
description: "Closes a working session as the last step of the work rather than stopping mid-air: runnable work committed, the operating state restored or named out loud, the next step left in words, and every decision that exists only in the conversation written somewhere that outlives it. Use when a session is ending for any reason — you were asked to stop, time ran out, the context window is filling — when you are handing a piece of work to a later session or to another agent, when you have paused a timer, opened a maintenance window or switched a check off during the work, when a decision was taken in conversation and is written down nowhere, and when the session is nearly over and nothing has been committed. The trigger is the signal rather than the word: reacting only to one particular phrase implements this rule at its least important point. Not for checking whether a claim you are about to write into the handover is true (use measure-before-asserting), not for recording where a fact came from (use provenance-on-entry), and not for deciding who picks the work up next (use delegation-contract)."
---

# Session handover

**Closing a session is the last step of the work, not the end of it.** Whatever exists only in
the conversation stops existing when the conversation does, and that is true whether the
session ends because somebody said so, because time ran out, or because the context window
filled up.

So the close is work, and it has a deliverable: a state somebody — possibly you, with no
memory of this — can pick up.

## What is lost if nobody writes it down

Four things, and none of them can be recovered by looking. Each has to be worked out again.

- **Half-finished changes in the working tree.** At the next start they look like an
  unexplained intermediate state, and nothing says whether they are deliberate.
- **The changed operating state.** A paused timer, an opened maintenance window, a check
  switched off to get something done. These do not announce themselves.
- **The next step.** What would be done next and *why that*. The most expensive item in the
  list, because it is the product of thinking rather than of measurement.
- **Decisions taken and recorded nowhere.** They come back up at the next run, and the person
  has to answer the same question twice — which is worse than merely wasteful, because the
  second answer may differ from the first and nothing will notice.

**The rule:** before a session ends, make the state durable. Runnable work committed. The
operating state restored, or named explicitly where it cannot be. The resumption point left in
words. Every decision that lives only in the transcript written to the place it belongs.

Where the close leaves a command for a person to run, show it **visibly and one at a time**
rather than bundled into a paragraph. A list of four commands in prose is read as background.

## Two of the three kinds of state are silent

This is the asymmetry that makes the rule counter-intuitive, and it is why the item everybody
does is the least valuable one.

Uncommitted files are **visible**: the tooling announces them, and the next session trips over
them immediately. A changed operating state is **silent** — a paused timer does not report that
it is paused, and a suppressed check looks exactly like a check that passed. An unwritten
decision is **silent** in the same way, and worse, because nothing anywhere records that it was
ever taken.

The consequence: the one loss class that has tooling behind it is the one you would have caught
anyway, and the two with nothing behind them are the ones a habit has to carry. If a handover
routine only ever produces a commit, it has addressed the class that needed it least.

## Why this is an invariant and not a courtesy

A system whose sessions hand over is the same system across any number of sessions. One whose
sessions simply stop decomposes into unconnected episodes, and every episode begins with
reconstruction.

The arithmetic is the whole argument. **The cost of handing over is constant and small. The
cost of reconstruction grows with the gap and is never complete** — some of what was in the
previous session is not recoverable at any price, only re-decidable, and re-deciding is how a
system quietly changes direction without anyone choosing to.

## The signal, not the word

"Stop", "that's enough for today", a terminal instruction, a quiet trailing-off, a context
window with little left in it: all of these are the same signal. Implementing this rule as a
trigger on a particular phrase implements it at its least important point, and it will be right
most of the time, which is what keeps the gap open.

The reliable form is to ask the question at the moment the *work* changes shape rather than at
the moment a particular sentence arrives: am I about to stop being able to add to this?

## A case of this shape

Told as a shape, not as an incident — it is the failure the rule is built against, and an
example that presents itself as an event owes a source it does not have here.

A session pauses a scheduled job so that a long operation can run without interference, and
switches off one check that was firing on a known-good intermediate state. The work goes well.
The session ends on a commit, tidily, and the commit is genuinely complete.

Days pass. The scheduled job has not run, and nothing reports that, because a job that is not
running produces no output — which is indistinguishable from a job that ran and found nothing.
The check is still off, so the class of problem it existed to catch accumulates in silence. The
next session sees a clean tree, a green status and no open question, and has no reason to look
for either.

**The commit was the visible item. The two silent ones were the whole cost.** Both would have
been a sentence each in a handover note, and neither is discoverable afterwards by any amount
of reading the repository.

## What actually enforces this

| Rule | Enforcement |
|---|---|
| Runnable work is committed before the session ends | **Enforceable, and partly enforced where it was measured — but the coverage is the finding, not the mechanism.** A start-time check that refuses to proceed on a dirty tree is real and exists. Where this was measured, it watched a fixed list of eight path entries covering tooling and configuration, and nothing about the recorded content: the knowledge a session actually produces sat outside its field of view entirely. It also existed in only one of the repositories involved and in none of the others, which is the ordinary state of a gate that was built for one surface and describes itself as protecting sessions. |
| A handover note **exists** | **Enforceable, not enforced**, and this is the cheapest unbuilt mechanism in the set: something that runs at the close can look for a note dated today and refuse, or at minimum complain, when there is none. Nothing here does it. Saying "no mechanism is possible" about this row would be the over-cautious kind of false assurance — the kind nobody ever audits, because it only slows things down. |
| The handover note is **useful** | **Behaviour rule, and no mechanism is possible.** No checker can determine whether a resumption point helps the next reader; it can determine only whether one was left at all. Checkability ends at presence, and the quality stays judgement work. The two rows together are the honest split: the existence of a note is a mechanism waiting to be written, and its worth is not. |
| The operating state is restored, or named where it cannot be | **Behaviour rule.** Nothing enumerates what a session changed about its own environment. This is the silent class, and it has no mechanism anywhere in sight. |
| Decisions that exist only in the conversation are written down | **Behaviour rule.** Nothing can tell that a decision was taken, so nothing can tell that one is missing. An absent record and a session in which nothing was decided are the same text. |
| The close triggers on the signal rather than on a phrase | **Behaviour rule**, and one that a mechanism would actively damage: a trigger on a list of phrases is buildable, would fire reliably, and would establish exactly the narrow reading the rule warns against. |
| Commands left for a person are shown singly | **Behaviour rule.** |

*Measured 2026-09-15: this repository contains prose, one plugin manifest and one pre-push
script; that script runs when something is pushed, checks commit identity and personal-data
shapes, and has nothing to do with sessions. Nothing here observes a session ending. The
dirty-tree gate described in the first row was read in the system this rule came from, where
its watched surface is a list of eight entries naming tooling and configuration paths.
Re-check by 2026-12-15.*

## A cheap check before a session ends

- Is anything uncommitted that will look like an unexplained intermediate state tomorrow?
- What did I change about the environment that will not announce itself — a paused job, a
  suppressed check, a widened permission, an open window?
- What is the next step, and *why that one* rather than another? If I cannot say why, the note
  is a task list rather than a handover.
- Was anything decided here that exists nowhere but this conversation?
- Is there a command somebody has to run? Then it is shown on its own, not inside a sentence.
- Am I treating "no one said stop" as evidence that the session is not ending?
