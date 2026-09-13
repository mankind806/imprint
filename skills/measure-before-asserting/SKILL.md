---
name: measure-before-asserting
description: "Runs, queries or looks the thing up before saying it, and treats your own notes and documentation as a dated snapshot rather than as evidence. Use before stating any property of a system — whether a repository is public, which version is running, what a config actually contains, whether a gate really blocks, whether a path exists, how large something is — before writing a claim into a document or a rule, whenever an assurance sounds careful and was never checked, and when a measurement has just contradicted something you wrote down earlier. Not for deciding who reviews the claim (use blind-first-pass) and not for the mechanics of dispatching the check (use delegation-contract)."
---

# Measure before asserting

**Anything that can be executed, queried or looked up is executed, queried or looked up
before it is said.** This covers properties of foreign systems and your own inventory alike:
whether a repository is public, which version is running, what a configuration file
contains, whether an enforcement actually fires, whether a path exists, how big something
is.

The cost is a few seconds. The alternative is a statement that sounds right.

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
seconds: both private. In the same pass a second assurance fell — a permission setting
described as configured existed in none of the three effective configurations.

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

Nothing, and a tool is not possible. No checker can determine whether a spoken claim was
measured beforehand — it sees only the result, and a guessed value can happen to be right.
This is an explicit **behaviour rule**.

What *is* measurable is the stock: **statements about foreign surfaces in your own documents
that carry no measurement date.** That number can be counted, tracked and driven down. Pick
that as the metric rather than "did the agent check" — the first is observable and the second
is not.

## A cheap check before you assert

- Can this be executed, queried or looked up in under a minute? Then it must be, now.
- Am I about to quote my own notes? Say so, and name their date.
- Does this claim feel obviously true? That is the trigger, not the exemption.
- Am I about to write an assurance? Then also write down what it does **not** cover.
- If I cannot measure it, do I say "unmeasured" rather than picking the cautious-sounding
  answer?
