---
name: measure-before-asserting
description: "Runs, queries or looks the thing up before saying it, and treats your own notes and documentation as a dated snapshot rather than as evidence. Use before asserting a property of a system you do not control (whether a repository is public, what a provider's terms now say, which version is running), before describing what one of your own safeguards covers or blocks, before quoting a state that can change without your involvement (what a config contains, whether a gate still fires, what a path holds), and before any claim of those kinds is written into a rule, a README or a handover. Also when an assurance sounds careful and was never checked, and when a measurement has just contradicted something you wrote down earlier. Not for deciding who reviews the claim (use blind-first-pass), not for the mechanics of dispatching the check (use delegation-contract), not for the mechanics of closing a session so the next one can pick the work up (use session-handover), and not for how long a stored look-up stays usable or how to record a re-check that found nothing changed (use knowledge-ages) — the division between those two is that a value a foreign organisation sets is never answered from storage at any age, which is this skill, while a value nobody sets individually ages on a horizon, which is that one. Recording where a measurement came from once you have it belongs to provenance-on-entry."
---

# Measure before asserting

**Anything that can be executed, queried or looked up is executed, queried or looked up
before it is said.** This covers properties of foreign systems and your own inventory alike:
whether a repository is public, which version is running, what a configuration file
contains, whether an enforcement actually fires, whether a path exists, how big something
is.

The cost is a few seconds. The alternative is a statement that sounds right.

## Where this bites, and where it does not

A rule that fires on every sentence gets ignored, which is the same outcome as not having
one. Three kinds of claim are worth the interruption:

- **A property of a system you do not control.** Someone else may have changed it since you
  last looked, and they owed you no notice.
- **What one of your own safeguards actually covers.** This is the class nobody audits; the
  section below on over-cautious assurances is about why.
- **A state that can change without your involvement** — a configuration, a version, a
  permission, a path.

A claim of any of those kinds that is about to be **written down** is the strongest trigger
of all, because writing is what converts it into something other people will quote
instead of check. Arithmetic you just did, a preference, a judgement call about what to
build next: not this skill's business.

## The values that are never answered from storage

One class of value deserves a procedure rather than a preference, and the question that
identifies it is **who is entitled to change this value?**

If a single outside body sets it and may change it without telling you — an authority, an
institution, a provider stating its own terms — then **no stored age is acceptable**, not even
a day. Published conditions, a stated term, a documented procedure, a price, a timetable, an
availability, who is responsible for which part: anything one body announces and can
re-announce without consulting you belongs here. In the rulebook this was drawn from that list
is binding rather than illustrative, and yours is worth writing down as a list too — a
criterion re-applied from memory each time decides borderline cases inconsistently.

Four steps, and the order is part of the rule:

1. **Consult the stored state silently.** Saying it first is the mistake, not saying it at
   all: a figure that has been spoken lands, and a caveat after it does not retract it.
2. **Look it up now**, before answering.
3. **If the two differ, give both**, mark the looked-up one as the one that governs, keep the
   retrieved source, and **supersede the stored entry in the same move** — not afterwards, not
   as a note to self. Skipping this is the quiet failure: the answer is right, and the store
   still holds the wrong value for whoever reads it next.
4. **If nothing is found**, give the stored state *with its age*. An entry carrying no date is
   reported as undated. Never estimate one.

Say what the source does not settle. That often yields "not answerable" rather than a figure,
and "not answerable" is an answer.

**Where the stored state governs and the published one does not** — these follow from the same
criterion rather than from a list to be memorised: **anything you were a party to.** Terms
fixed by an agreement you hold, because the conditions published today govern whoever agrees
today and not an agreement already made. Your own arrangements, amounts from invoices in your
possession, measurements you took yourself. Values your own store actively monitors. And
private individuals, who are not looked up at all. If nobody outside could have changed it
without your involvement, the store is the better source and the public page is the wrong one.

What drifts on a horizon instead of falling under this section — anything no single body sets
— belongs to `knowledge-ages`.

## Your own documentation is not a measurement

It is a compilation with a date on it. What is written there may have become false since it
was entered, or may have been false from the start — and **both look identical when you read
it**. Quoting a property from your own rulebook is not checking it; it is passing it on.

The same applies to a comment, a README, a previous agent's summary, and the sentence you
yourself wrote three turns ago.

## The failure mode is plausibility, not ignorance

You do not skip the check because you do not know. You skip it because the claim fits.

A worked case: a rulebook stated in five places that two code repositories were public, and
the system's sharpest safeguard was derived from that — "the channel with the highest
consequence," "not retractable," "only on request." The statement had been internally
consistent for years, matched how everyone spoke about it, and produced a coherent picture.
**That is exactly why nobody checked it.** One query to the provider disproved it in two
seconds: both private. In the same pass a second assurance fell the same way: a safeguard
the notes described as in place was not in place anywhere that would have had to carry it.
Nobody had opened the configurations that would have had to carry it, because a safeguard
that only slows things down is the last thing anyone thinks to audit — the next section is
about that class.

Nothing about that story is unusual. A false statement that contradicts its surroundings
gets caught. A false statement that fits does not.

## Over-cautious false statements are the dangerous class

An over-optimistic assurance fails on the first real run. An over-cautious one looks like
prudence and is never doubted — nobody audits a safeguard that only slows things down.

Its damage is real anyway. It prevents correct work, it points attention at the harmless
channel instead of the dangerous one, and when it finally falls it takes the whole rule with
it — including the parts that were justified for other reasons.

So the direction of an error tells you how it will be found. Only one of the two directions
is self-correcting.

## When a measurement refutes a rule

Correct the fact immediately. Do **not** drop the protective rule until you have checked its
*remaining* justifications. Usually it still holds — just for a different reason, and that
reason now needs to be written down explicitly.

The reverse is also true: a rule that turns out to rest on nothing but a false premise should
be retired out loud, not quietly left standing to be obeyed for no reason.

## Adjacent habits that come from the same root

**A named gap without a date becomes a property.** Wherever your text says "not enforced,"
"unmeasured," or "assumed," put a re-check date next to it. Without one, the gap stops being
a gap and turns into how the system simply is.

**A dated check note about a foreign surface goes stale silently.** "Checked on «date»"
describes a state that moves without your involvement. The remedy is not a more diligent list
but a checker — and until that exists, a sentence that says out loud the note will expire.

**Internal consistency is not a correctness proof.** A result can be contradiction-free and
wrong. It needs an independently computed comparison value; if there is none, that absence is
the finding.

**A retry is not a diagnostic instrument.** It hides the persistent cause. Whoever retries
names the time-dependent cause they are assuming first; if they cannot name one, looking is
the correct action.

## What actually enforces this

The central rule is a behaviour rule and that will not change. But this skill also carries a
procedure now, and a procedure has parts that a mechanism reaches — so the section is a table
like every other one here rather than a single sentence that writes all of it off.

| Rule | Enforcement |
|---|---|
| A claim was measured before it was spoken | **Behaviour rule**, and no mechanism is possible. No checker can determine whether a spoken claim was measured beforehand — it sees only the result, and a guessed value can happen to be right. This row is the reason this section used to say "nothing", and it is the only row for which that answer was ever correct. |
| The class of values never answered from storage is written down as a list | **Enforceable, not enforced**. A criterion re-applied from memory decides borderline cases inconsistently, which is why the section above asks for a list rather than a habit. A list is a file; a retrieval path can read it and mark an entry as belonging to the class. Nothing here ships one, and the rule deliberately stops at the shape, because a list naming real authorities would go stale faster here than in the system using it. |
| Statements about foreign surfaces carry a measurement date | **Enforceable, not enforced**, and this is the cheapest row: scanning your own documents for an assurance about something you do not control that carries no date is a short script, and the result is a number that can be tracked and driven down. Pick that as the metric rather than "did the agent check" — the first is observable and the second is not. |
| A look-up that differs from the store supersedes the entry in the same move | **Enforceable, not enforced**. The quiet failure is an answer that is right while the store keeps the wrong value for the next reader. A write path that will not close a corrected answer without also writing the replacement is a real mechanism; so is a check that lists entries contradicted by a retrieval recorded later than they were. Neither exists here. |
| The stored state is consulted silently, before the look-up rather than after | **Behaviour rule**. The order is the rule, and nothing observes the order in which a party thought. A figure once spoken lands, and a caveat after it does not retract it — which is exactly the kind of failure no log shows. |
| An assurance says what it does **not** cover | **Behaviour rule**. Whether a stated limit is the real one is a judgement about meaning. A checker can see that a sentence about coverage exists; it cannot see that it is honest. |
| An unmeasurable thing is reported as unmeasured rather than guessed cautiously | **Behaviour rule**, and the one this skill exists for. A cautious-sounding wrong answer and a measured right one are the same shape on the page, which is why the over-cautious assurance is the dangerous class — nothing ever makes it fail. |

*Measured 2026-09-15: nothing in this repository executes any of the enforceable rows. The one
script it ships runs at push time and checks commit identity and personal-data shapes.
Re-check by 2026-12-15.*

## A cheap check before you assert

- Can this be executed, queried or looked up in under a minute? Then it must be, now.
- Am I about to quote my own notes? Say so, and name their date.
- Does this claim feel obviously true? That is the trigger, not the exemption.
- Am I about to write an assurance? Then also write down what it does **not** cover.
- If I cannot measure it, do I say "unmeasured" rather than picking the cautious-sounding
  answer?
