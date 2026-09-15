---
name: session-handover
description: "Closes a working session as the last step of the work rather than stopping mid-air: runnable work committed by whoever holds the pen, the operating state restored or named out loud, the next step left in words, and every decision that exists only in the conversation written somewhere that outlives it. Use when a session is ending — you were asked to stop, the context window is near its limit, a delegated task is returning, the agreed piece of work has no next item — when you have paused a timer, opened a maintenance window or switched a check off during the work, when a decision was taken in conversation and is written down nowhere, and when the session is nearly over and nothing has been committed. A session ends when the work can no longer be added to, which is not the end of every turn; and what the close then does depends on the role, because an advisor reports the uncommitted state rather than committing it. The trigger is the signal rather than the word: reacting only to one particular phrase implements this rule at its least important point. Handing work to another agent fires this skill and delegation-contract together, which is correct — this one is the content of what gets handed over, that one is the envelope and who ends up holding the pen. Not for checking whether a claim you are about to write into the handover is true (use measure-before-asserting), and not for recording where a fact came from (use provenance-on-entry)."
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
Three of the four are silent, which is the asymmetry the section after next is about.

- **Half-finished changes in the working tree.** At the next start they look like an
  unexplained intermediate state, and nothing says whether they are deliberate.
- **The changed operating state.** A paused timer, an opened maintenance window, a check
  switched off to get something done. These do not announce themselves.
- **The next step.** What would be done next and *why that*. The most expensive item in the
  list, because it is the product of thinking rather than of measurement.
- **Decisions taken and recorded nowhere.** They come back up at the next run, and the person
  has to answer the same question twice — which is worse than merely wasteful, because the
  second answer may differ from the first and nothing will notice.

**The rule:** before a session ends, make the state durable. Runnable work committed — by
whoever holds the pen. An advisor with no write permission does not acquire it here: it
*reports* the uncommitted state, names the paths, and leaves the commit to the party that
holds the write right. A handover that breaks the one-writer invariant to satisfy itself has
cost more than it saved; `delegation-contract` holds that invariant. The operating state
restored, or named explicitly where it cannot be. The resumption point left in words. Every
decision that lives only in the transcript written to the place it belongs.

Where the close leaves a command for a person to run, show it **visibly and one at a time**
rather than bundled into a paragraph. A list of four commands in prose is read as background.

## Only one of the four losses is visible

This is the asymmetry that makes the rule counter-intuitive, and it is why the item everybody
does is the least valuable one.

Of the four, exactly one announces itself. Uncommitted files are **visible**: the tooling
announces them, and the next session trips over them immediately. Of the three kinds of
*state* the list names, two are silent — a paused timer does not report that it is paused, and
a suppressed check looks exactly like a check that passed; an unwritten decision is silent in
the same way, and worse, because nothing anywhere records that it was ever taken. The fourth
loss is not a state at all but the reasoning — the next step and *why that one* — and it is
the quietest of the lot, because nothing was ever there to fall silent.

The consequence: the one loss class that has tooling behind it is the one you would have caught
anyway, and the three with nothing behind them are the ones a habit has to carry. If a handover
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

**A signal has to be nameable or the rule never fires reliably.** "Ask at the right moment" with
no observable attached is a rule that is either never triggered or triggered at every turn. So
name the moments. These four are observable without judgement, and the question is asked at
each of them as a minimum:

- **An explicit terminal instruction**, in whatever words — the easy case, and the only one a
  phrase trigger would catch.
- **A context window near its limit**, on whatever indicator the harness exposes. The threshold
  is the point at which the close itself would no longer fit: a handover you cannot finish is
  not a handover, so the trigger has to come before the last usable stretch rather than at
  exhaustion. Where the harness signals an imminent compaction, that signal *is* the threshold.
- **A delegated task reaching its return.** For a subagent the return is the session end; there
  is no later moment.
- **The last item of the agreed piece of work being done**, with nothing named to follow it.

The question is what these trigger; the *action* still depends on who you are. An advisor's
close is a report, a leading agent's close includes the commit — the asking is unconditional,
the committing is not.

What stays judgement is only the residue: a quiet trailing-off, an ambiguous pause. The four
above are the floor, not the ceiling, and a rule with a floor is one that can be checked.

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
| Runnable work is committed before the session ends | **Enforceable, not enforced** here, and enforced in the system this rule was read in — where the coverage, not the mechanism, is the finding. A check at the session's start that refuses to run on a dirty tree is real and exists there; so does one at the end of every turn. What both watch is a short fixed list of tooling and configuration paths, and nothing about the recorded content: the knowledge a session actually produces sits outside their field of view entirely. They existed in one repository and not in its siblings, which is the ordinary state of a gate built for one surface that describes itself as protecting sessions. |
| A handover note **exists** | **Enforceable, not enforced**, and this is the cheapest unbuilt mechanism in the set: something that runs at the close can look for a note dated today and refuse, or at minimum complain, when there is none. Nothing here does it. Saying "no mechanism is possible" about this row would be the over-cautious kind of false assurance — the kind nobody ever audits, because it only slows things down. |
| The handover note is **useful** | **Enforceable, not enforced** for the note's form; **Behaviour rule** for its worth. This skill defines four parts a note has — what is open, what changed in the environment, the next step *with its reason*, and the decisions taken — and a checker for four non-empty parts is short. That catches the specific failure named in the cheap check below, a note that is a task list because the reasons are missing. Whether the note actually *helps* the next reader no mechanism reaches. Saying the whole row is impossible concedes the cheap half with the expensive one. |
| The operating state is restored, or named where it cannot be | **Enforceable, not enforced**, and this row read "no mechanism anywhere in sight" until somebody looked. The mechanism is a ledger: every suspension of a check or a schedule is entered with a path or a name **and an expiry**, and the close reads the ledger back. In the system this rule was read in, exactly that exists — suspensions are enumerated, dated and expiring, and the turn's end reports them along with the time the exemption runs out. What stays unenforced is the entering: a suspension made without going through the ledger is invisible to it, the same shape a write path has when a file is opened in an editor instead. |
| Decisions that exist only in the conversation are written down | **Behaviour rule**. Nothing can tell that a decision was taken, so nothing can tell that one is missing. An absent record and a session in which nothing was decided are the same text. Unlike the two rows this round corrected, this one has been looked for and there is genuinely nothing. |
| The close triggers on the signal rather than on a phrase | **Enforceable, not enforced** for the observable signals; **Behaviour rule** for the judgement inside them. This row was wrong in the more dangerous direction. An explicit stop, a context window near its limit, a delegated task returning, and a session ending are events a harness can name: the runtime measured below names events for an end of turn, an imminent compaction, a subagent's return and a session's end, which bracket the observables the section above lists. A check that fires on the *event* rather than on a word neither narrows the reading nor needs a phrase list. One such gate, on the end of a turn, is running in the system this rule was read in. What no mechanism reaches is only the judgement inside the observable: is this the end? The original objection — that a phrase trigger would establish the narrow reading — is true of a phrase trigger and was wrongly generalised to every mechanism. |
| Commands left for a person are shown singly | **Behaviour rule**. |

*Measured 2026-09-15: nothing in this repository observes a session ending. The one script it
ships runs when something is pushed, checks commit identity and personal-data shapes, and has
nothing to do with sessions. The gates named in the rows above were read in the system this
rule was read in, as source rather than as documentation: one wrapping the jobs a session runs
at its start, which exits non-zero on a dirty tree so that those jobs do not run — the session
itself is not its subject — and one on the end of a turn, which blocks the stop once so the
commit happens while the session that caused it is still there. Both watch the same short list
of tooling and configuration paths. The suspension ledger named in the fourth row was read in
the same place: entries carrying a prefix and an expiry, reported back at the turn's end.
Separately, the harness running this: its hook events include an end of turn, an imminent
context compaction, a subagent's return and a session's end — read as event names in the
binary of the version in use, not executed, so what is established is that such events are
named and not what a handler may do in them. Re-check by 2026-12-15.*

**Two rows in this table said a mechanism was impossible or harmful, and both were wrong in
the direction that never gets audited.** An assurance that something cannot be built only ever
slows people down, so nobody goes looking for the counterexample — and both counterexamples
were running in the system the rows cite as their source. The rule that follows from it is
narrow and belongs here rather than in a postscript: **a row that says no mechanism exists owes
a sentence about where it looked.** If it cannot name the search, it is a guess wearing the
clothes of a finding.

## A cheap check before a session ends

- Is anything uncommitted that will look like an unexplained intermediate state tomorrow?
- What did I change about the environment that will not announce itself — a paused job, a
  suppressed check, a widened permission, an open window?
- What is the next step, and *why that one* rather than another? If I cannot say why, the note
  is a task list rather than a handover.
- Was anything decided here that exists nowhere but this conversation?
- Is there a command somebody has to run? Then it is shown on its own, not inside a sentence.
- Am I treating "no one said stop" as evidence that the session is not ending?
- Do I hold the write right? If not, the close is a report of what is uncommitted and where,
  not a commit.
