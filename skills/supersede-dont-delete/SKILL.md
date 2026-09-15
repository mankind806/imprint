---
name: supersede-dont-delete
description: "Replaces an outdated entry instead of overwriting it, so the state that was replaced stays provable and dated after the visible one changes, and stops a process rather than resolving it when a new statement contradicts a stored one. Use when a stored value has changed and you are about to type over it, when an item is complete and you are about to delete its line, when you are tempted to write the completion and the follow-up step into one line, when a new statement disagrees with what is already recorded and taking the newer one looks obvious, when you have just found a second stored copy of a fact that disagrees with the first, when you need to know later whether an old value was wrong or was right at the time, and when a convention for recording replacements has changed and a stock of older entries is tempting you into a migration. Not for re-dating an entry that turned out to be unchanged, which is not a replacement at all (use knowledge-ages), not for deciding which place holds the value being replaced (use one-canonical-place), not for recording where the replacing value came from (use provenance-on-entry) and not for the values that are never answered from storage at all, where a difference is resolved by looking rather than by asking (use measure-before-asserting), and not for what a closing session owes the next one (use session-handover)."
---

# Supersede, don't delete

**An outdated state is superseded and stays provable. Only the presentation changed.** The
living line carries the value that holds now and a pointer to what it replaced; the replaced
version stands with its date where it can still be read.

Deleting is faster and loses the one thing the entry was for.

## What the history is actually for

Not completeness, and not an audit trail for its own sake. It answers one question that comes
up constantly and cannot be answered any other way: **was the old value wrong, or was it
right at the time and then changed?**

Those two have nothing in common. One means an error to trace — somebody recorded something
incorrectly, and whatever else came from that source is now suspect. The other means a
normal change, and the old value was good evidence about an earlier state. Acting on the
wrong reading wastes a day in one direction and repeats a mistake in the other.

An overwrite destroys the distinction completely and leaves a value that looks perfectly
trustworthy. **The information was not wrong afterwards. It was absent, and nothing said so.**

## A case of this shape — told as a shape, because that is all it is

Nothing below is a report of an incident; it is the shape the failure takes, and it is written
as a shape rather than dressed up as a dated event. A worked case that cannot name its source
should say so, which is `provenance-on-entry` applied to this file.

A figure needs updating. It is one line in one file, so the new value is typed over the old
one and the file is saved. This is the correct-looking action and it takes four seconds.

Later the figure is disputed from another direction, and the question is exactly the one above
— was the earlier figure a mistake, or was it right at the time and then changed? The file
holds the new number and no trace of the old one. The commit history holds a diff, which is
better than nothing and is not the same thing: it says the text changed on a date, not whether
the change was a correction or an update, and it is not where anybody reading the knowledge
looks.

The repair is to go back to the source and re-derive both states, which costs far more than the
four seconds saved — and only works at all while the source still exists.

## The second worked case is one line long

A completed item and the step that follows from it get written into the same line: done, and
then what happens next. Later the line is treated as complete — because it says so — and the
follow-up disappears with it. Nobody deleted the follow-up; it was never separately visible.

**Completion and the next step are two lines, never one.** A line has one state, and if you
give it two, the reader gets whichever one is at the front.

## Form is local; one property is not

Stores differ in how they carry a replaced state — a dated note at the foot of the entry, the
old text struck through in the line itself, a version field. Any of these satisfies the rule.
What has to hold is the property: **from the living line, a reader can reach the replaced
state, and it carries a date.**

Two consequences worth stating, because both get decided wrongly:

- **A change of convention is not a reason to migrate a stock.** Where older entries use an
  older form, they stay valid and stay as they are. Rewriting them costs real risk for no
  gain in provability — the old form already provides it — and a migration is precisely the
  operation during which histories get flattened.
- **Where a value is authoritative for a whole area, the replacement covers the area, not
  just the line you happened to open.** That is `one-canonical-place` arriving from the other
  side: if superseding one value leaves a second copy holding the old one, the supersession
  produced a contradiction rather than resolving one.

## A contradiction stops the process

When a new statement disagrees with a stored one and it is **not clear which holds**, the
correct action is not to take the newer one. It is to change nothing, show both states in
full, and let a person decide.

**Two stored copies that disagree are this case, not a separate one.** Finding the same fact
in two places holding different values looks like a duplication to clear up, and clearing it
up means choosing one — which is the decision this rule reserves. `one-canonical-place` says
where the fact belongs; it does not say which of the two values is true, and a copy sitting in
the right place is not thereby the current one. Stop first, align afterwards. The process rests until then, and the open point is
additionally kept visible as its own line — the stop halts the work, the line keeps it from
being forgotten once the work resumes.

Taking the newer value is the tempting error because it is usually right. The reason it is
still wrong is that "usually right" silently discards the case where the new source is the
faulty one, and that case leaves no evidence behind once the old value is gone.

**What is not a contradiction in this sense**, and this boundary does real work: if the
value is one whose **change-authority is settled** — somebody or some body is entitled to
change it, and did — then a differing value is an **update**, not a contradiction. It gets
superseded on the spot with no question asked. The question exists for genuine uncertainty
about which state is true, not for every difference. `knowledge-ages` carries the criterion
that tells the two apart, and `measure-before-asserting` covers the values that should never
have been answered from storage in the first place.

Without that boundary the stop rule fires on every routine update, and a rule that stops
everything gets switched off.

## What actually enforces this

This plugin ships nothing that runs when you write. The states below say what is possible —
and one of them, **reserved to a person**, exists because of the last rule in this skill.

| Rule | Enforcement |
|---|---|
| Immutable sources are never modified after they are first committed | **Enforceable, not enforced**. This is the strongest mechanism available anywhere in the knowledge layer: a commit-time check that refuses a modification or deletion under a path prefix is short, decidable and has no judgement in it. Nothing here implements it. |
| A superseded value cannot be erased through the tool | **Enforceable, not enforced** — and worth noting for its shape: the achievable version is not a check that complains but a write path that **offers no operation** for removing a replaced state. A mechanism that cannot express the wrong thing beats one that detects it. |
| A value overwritten by editing the file directly keeps its history | **Behaviour rule**, and this is the gap the row above does not close. Whatever the tool refuses, an editor will do, and nothing observes it. |
| A knowledge page is not deleted outright | **Enforceable, not enforced** — the same mechanism as the row above with a different path prefix, and that is the whole argument: a commit-time check that refuses a deletion under a prefix does not care which prefix it is given. Where this was measured no gate prevented the deletion of a compiled page, which is a current state rather than an impossibility. It reads like the harder case because immutability is discussed for sources and never for pages. |
| Completion and follow-up are separate lines | **Behaviour rule**. |
| A central value is superseded everywhere it is authoritative | **Behaviour rule**, for the same reason duplication is undetectable — see `one-canonical-place`. |
| A contradiction is noticed rather than merged away | **Behaviour rule**. Nothing recognises a contradiction, because recognising one is the judgement the rule is about. |
| A noticed contradiction is resolved by a person rather than by taking the newer value | **Reserved to a person**. Not an unbuilt mechanism — the rule's content *is* the person's decision. See below, including why the rulebook this came from does not file it here. |

*Measured 2026-09-15: the only script this repository ships runs at push time and checks
commit identity and personal-data shapes. Neither of the "enforceable" rows above exists here.
Re-check by 2026-12-15.*

## The fourth state, and why the rulebook this came from does not use it here

This repository sorts rules into four states: enforced, enforceable but not enforced,
**reserved to a person**, and plain behaviour rule. The fourth exists because of the
contradiction rule, and the reason is worth the paragraph.

The stop is not enforced, and it is not usefully *enforceable* either — no mechanism can
recognise a contradiction, because recognising one is the judgement the rule is about. But
calling the whole rule a behaviour rule says "nothing holds this but discipline", which is true
about **noticing** the contradiction and false about what happens next. What happens next is
not an absent mechanism. **The rule's content is a person's decision**, and that content *is* the
decision point itself: change nothing, show both, wait.

The two fail differently, which is why one cell cannot carry both. An unbuilt mechanism fails
by never being built. A consent step fails by somebody deciding it was obvious enough to skip.
Only the first is repaired by writing code, and a table that files them together tells you to
write code for the second.

**Here this skill classifies something its source does not, and that is said rather than
smoothed over.** The rulebook this rule was drawn from has a consent stage of its own — it uses
it for the points where an action waits on a human yes, and this rule is not one of them there.
It rates the contradiction rule at the behaviour stage and records the human decision as the
*consequence* of a finding rather than as the thing being enforced, with the explicit reach
that noticing a contradiction is judgement work and enforced by nothing whatsoever. That
reading is coherent: what is unenforced is the noticing, and the decision is where unenforced
noticing leads.

So the table above splits the rule in two rather than picking a side, which is the only form
true to both readings — noticing is a behaviour rule, resolving is reserved to a person. What
does not get glossed is that the second row is a classification made **here** and not carried
over, and that a reader comparing this skill against its source will find the rule filed one
stage lower there. Wherever a rule ends in "ask first", expect this split rather than a single
cell.

## A cheap check before you change a stored value

- Am I typing over something? Where does the old value go?
- If somebody asks next month whether the old value was wrong or was superseded, what answers
  them?
- Does this line say both "done" and "next"? Then it is two lines.
- Is this actually a contradiction, or an update by somebody entitled to make it? If it is a
  contradiction: stop, show both, ask.
- Is this value authoritative for more than the page I opened?
