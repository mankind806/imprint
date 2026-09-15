# imprint

A suite of working rules for two things that turn out to be one thing: how several AI
coding agents cooperate without ruining each other's work, and how a knowledge base grows
alongside the person using it without quietly rotting.

Both are written as mechanisms rather than as advice, and every rule says which of four
things it is: enforced by something that stops you, *enforceable* but currently resting on
discipline, reserved to a person because the rule's content *is* a human decision, or a
behaviour rule with nothing behind it at all.

It ships as a Claude Code plugin, so the rules arrive as skills the agent can reach for
during a session rather than as a document somebody has to remember to open.

There is an awkwardness in that sentence worth saying out loud rather than burying at the
bottom. A skill loads when the model recognises that the task matches it — which is *after*
the model has decided how to approach the task. But the delegation contract is mostly about
decisions taken before that point: who leads, whether to dispatch at all, who ends up
holding the pen. By the time a model thinks "this is a delegation question, I should open
the delegation skill," it has usually already framed the delegation. A plugin cannot ship a
`CLAUDE.md` (see [Known limits](#known-limits)), so there is no ambient-context route
either. The foundation layer is the one the delivery mechanism guarantees least. Until that
changes, the honest reading is: these skills work best when you invoke them deliberately at
the start of a piece of multi-agent work, and only incidentally when the model happens to
reach for one mid-flight.

## Status

Early, and deliberately partial.

**Both layers now ship.** Four rules in the agent-collaboration layer, four in the
knowledge layer: the three that the section below announced as shape rather than content,
plus two for failure modes named under *Who this is for* — a fact that was true when it was
recorded and is still being quoted as current, and a session that ended without leaving a way
back in. Of the other two things that section announced, one was already here, and one turned
out to be half delivered and half wrong; both are dealt with where they stand rather than
quietly dropped.

That does not make either layer complete. It makes the announced part of it real.

The skills here were practice before they were text, which is the right order but means the
text lags the practice. **All eight have now had at least one adversarial read by a party that
did not write them** — the three agent-collaboration skills earliest, the four knowledge
skills on 2026-09-15, and `session-handover` later the same day, in a round of its own that
also re-read the seven around it.

This repository's own rule is that zero findings in a first adversarial round on a non-trivial
artefact is itself a finding, and no round here has come close to zero. What those rounds are
worth saying is not how many were closed but **what is still open**:

- **Whether any limit truncates a skill description.** Every description in this release runs
  past a thousand characters; the longest is a little over eighteen hundred. That they load has
  been observed in one runtime. No specification for a limit has been located either way.
  *Re-check by 2026-12-15.*
- **The remaining rows that say no mechanism exists have not been re-searched.** The most
  recent round found two places where this repository declared a mechanism impossible or
  harmful and was wrong both times, because nobody had gone looking — an over-cautious
  assurance is the kind nothing ever makes fail. Those two are corrected and now say where
  they looked. The other behaviour-rule rows across the eight skills carry no such sentence
  yet. *Re-check by 2026-12-15.*

Expect the structure to move again before it settles.

## Who this is for

Anyone working with more than one coding agent who has noticed that the interesting
failures are not bad code. They are two agents writing the same file at once, a fact that
was true when it was recorded and is still being quoted as current, a session that ended
without leaving a way back in, and an assurance that sounds careful and was never measured.

If you use a single assistant for single tasks, this is more machinery than you need.

## Installation

```
/plugin marketplace add mankind806/imprint
/plugin install imprint@imprint
```

The repository is its own marketplace, which is why the name appears twice. Both the
qualified form above and plain `/plugin install imprint` resolve to the same plugin, and
once installed the skills are addressed as `/imprint:delegation-contract` and so on, with
the agent as `imprint:foreign-material-reviewer`. Claude also reaches for them on its own
when a task matches their description.

*Measured 2026-09-13 on Claude Code 2.1.269, against a local checkout: `marketplace add`
followed by both the qualified and the bare `install` succeeded, and `claude
--plugin-dir … -p` lists all three skills and the agent under the `imprint:` prefix.* That
measurement stands as taken and the repository has grown past it: there were three skills
then and there are eight now. **Re-measured 2026-09-15 on Claude Code 2.1.272**, from an
isolated configuration directory, `--plugin-dir` against this tree at plugin version 0.3.0:
the `system/init` event lists all **eight** skills and the agent under the `imprint:` prefix.
That run ends in an authentication failure, which is expected and does not affect the
reading — the init event is emitted before the first API call, so what it shows is the tool
and skill set the harness composed rather than a model's account of it. Two further things
about the original measurement are worth stating rather than glossing:

- It exercised a **local path**, not the `mankind806/imprint` shorthand, which adds a clone
  step before the same resolution. That shorthand has since been measured on its own —
  2026-09-13, Claude Code 2.1.269, after the first push, from an isolated configuration.
  `marketplace add mankind806/imprint` cloned over HTTPS and validated, the qualified
  `install` reported success, and a session started afterwards listed all three skills — the
  three that existed on that date — and the agent, loaded out of the installed plugin cache
  rather than out of any checkout. Whether an installed copy of the present release lists all
  eight the same way is **not** separately measured; only the `--plugin-dir` path above is.
  *Re-check by 2026-12-15.* So a
  marketplace whose plugin `source` is the repository root does resolve over the network;
  had it not, the failure would have been at install time and total.
- Whether the bare `install imprint` stays unambiguous depends on the other marketplaces
  *you* have added. It was measured with no competing plugin of that name present, which is
  the only condition under which the shorthand is meaningful at all. If you already have an
  `imprint` from somewhere else, use the qualified form.

## What is in the box

Eight skills and one agent, in the two layers described below. Each skill carries a section
on what actually enforces it, and sorts every rule it holds into one of four states:
**enforced** by something that really stops you, **enforceable, not enforced** where a
mechanism is possible and nobody has built it, **reserved to a person** where the rule's
content *is* a person's decision rather than a mechanism nobody wrote, and a plain **behaviour
rule** that holds only as long as the discipline does.

The second state is the one usually left out, and it is the most useful one — it is a list of
the places where a few lines of tooling would pay. The fourth state started as three and gained
its extra member from a rule that would not fit: `supersede-dont-delete` argues for it at
length, including the fact that the rulebook these rules were drawn from files that particular
rule one stage lower, and why that is worth saying rather than smoothing over. The two states
fail differently — an unbuilt mechanism fails by never being built, a consent step fails by
somebody deciding it was obvious enough to skip — and only one of the two is repaired by
writing code.

- **`delegation-contract`** – who leads and who advises, and how a delegated task tells the
  receiver which of the two it is – including why the flag you were invoked with does not
  answer that question. The header to put in front of a dispatch. Readers in parallel,
  exactly one writer at a time, and where "do not do it yourself" stops. Why a subagent's
  own success report does not count as evidence that it wrote anything, and why a required
  human yes does not travel with a delegation. Which model the dispatch goes to once the
  size question is settled — mechanism downward, judgement upward, the cheapest one that
  clearly passes, and the condition that stops "cheapest" from meaning "under-provisioned".
  Why a roster of which model does which job needs a check date computed rather than stored,
  and goes stale silently without one.

- **`blind-first-pass`** – how to set up a second opinion so it is worth having. Staged
  disclosure: the reproduction and the numbers first, your hypothesis last. Why a
  context-inheriting dispatch silently gives you the opposite of a blind review. When a
  closed question wants an independent *measurement* rather than another opinion. How to put
  disagreement in front of a person as a table of claims instead of averaging it away.

- **`measure-before-asserting`** – anything that can be executed, queried or looked up is,
  before it is said. Your own notes are a snapshot with a date on them, not a measurement.
  The failure mode is plausibility rather than ignorance, and an over-cautious false
  assurance is the dangerous kind because nothing ever makes it fail.

- **`session-handover`** – closing a session as the last step of the work rather than stopping
  mid-air. The four things that are lost if nobody writes them down, and why the one everybody
  does is the least valuable: uncommitted files are visible, while a paused job and an unwritten
  decision are silent. Why the next step is the expensive item — it cannot be re-measured, only
  re-thought. Why the trigger is the signal rather than a particular phrase — a mechanism keyed
  on the phrase would make the rule worse, while one keyed on the *event* is buildable and
  named here, along with the four observable moments that are the floor. Where a handover
  note's *existence* and its *form* are mechanisms waiting to be written and its *usefulness*
  is not. And what the close does when you are the advisor rather than the one holding the pen.

The four that follow are the knowledge layer. They are written to be read in that order:
each one assumes the one before it, and the last is unusable without the second.

- **`one-canonical-place`** – one authoritative place per fact, every other view generated,
  linked or embedded rather than copied. Why aligning all the copies of a drifted value
  perpetuates the defect instead of repairing it, why the scope is any changeable
  fact rather than only numbers, and why the habit that actually carries the rule is a search
  performed before writing rather than a check performed afterwards.

- **`provenance-on-entry`** – where an entry came from, recorded when it is written, and
  *how* it was obtained as the load-bearing half: read, heard, measured, computed or
  inferred are five different futures for the same number. Why the date of recording is not
  the deadline and why one date in an entry that needs two looks complete rather than
  ambiguous. Why attribution collected at the foot of a page is attribution destroyed.

- **`supersede-dont-delete`** – replacing rather than overwriting, so that the one question
  an overwrite makes unanswerable stays answerable: was the old value wrong, or right at the
  time and then changed. Completion and follow-up as two lines. And a contradiction as a
  full stop rather than a merge — including the boundary that keeps that from firing on every
  routine update, and the argument for the fourth state, which this rule is the reason for.

- **`knowledge-ages`** – expiry triggered by use rather than by a schedule, with the
  dangerous entry being the recent-looking one. Why what ages is decided by the kind of
  evidence and not the kind of content, so a figure backed by a document does not age and a
  figure backed by a look-up does. The routing question — who is entitled to change this
  value — that separates what may expire from what must never be quoted from storage at all.
  Why confirming an unchanged value is not a replacement, measured from a case where
  recording it as one made the history report a change where unchangedness had just been
  established.

- **`foreign-material-reviewer`** (agent) – a read-only triage role for material you did not
  write. Its `tools:` frontmatter is an allowlist of `Read`, `Grep` and `Glob`, which is the
  point: a prompt asking an agent to stay read-only is a behaviour rule, and behaviour rules
  are broken by exactly the input this agent exists to handle. What it does *not* close is
  named in its own description.

  *That the allowlist is real was measured, not assumed — it is the strongest enforcement
  claim this repository makes, so it had to be. On 2026-09-13, Claude Code 2.1.269:
  the plugin was loaded with `--plugin-dir` under an isolated `CLAUDE_CONFIG_DIR`, and a
  session started with `--agent imprint:foreign-material-reviewer` reported
  `"tools":["Read","Grep","Glob"]` in its `system/init` event. The identical invocation
  without `--agent` reported twenty-three tools, among them `Task`, `Bash`, `Write`, `Edit`,
  `WebFetch` and `WebSearch` — so the narrowing comes from the frontmatter and not from the
  environment. The init event is emitted before the first API call, and that run never
  authenticated, so what it shows is the tool set the harness composed rather than a model's
  account of what it thought it had. Two things it does not show: it exercised the agent at
  **session scope**, and a subagent dispatch of the same definition is not separately
  measured; and `mcp_servers` was empty because the isolated configuration had none, which
  is no evidence either way about MCP tools being filtered. Re-check by 2026-12-13.*

## The two layers

**Agent collaboration is the foundation, and it shipped first.** Who leads and who advises,
and how a delegated task tells the receiver which of the two it is. Readers in parallel,
exactly one writer at a time. When a second opinion adds information and when it only adds
agreement – and why a closed, checkable question wants an independent measurement rather
than another opinion. How to put disagreement in front of a human instead of averaging it
away. Escalating to a stronger voice rather than resampling the same one after it has
already failed twice. And, at the other end of the same work, closing a session so that the
next one inherits a state rather than a puzzle — which belongs in this layer rather than the
one above it, because what a session hands over is the work itself and not the knowledge it
happened to record.

**A knowledge system sits on top, and four of its rules now ship.** One canonical place per
fact, with every other view generated, linked or embedded rather than copied. Provenance on
every entry, including how it was obtained, and a date something is due kept apart from the
date it was recorded. Superseding instead of deleting, so the replaced state stays provable
after the visible one changes. Knowledge that expires when it is reached for rather than on a
schedule, routed by the question of who is entitled to change a value. And a standing
preference for measuring a property over citing your own notes about it, which shipped first,
as `measure-before-asserting`.

**One item that stood in this list has been narrowed rather than built, because practice
refuted it.** It asked for *gates that stop an action rather than warn about it*, as a
universal. Where this was measured it is not one: an unattended run — a timer, a session
start, anything with no person in the loop — fails closed on a finding, while a supervised
run performs the same checks in full and only warns, deliberately, including where the
finding is in executable code. The defensible version of the rule is therefore narrower and
has a measurement in it: **a gate blocks when nobody is watching and warns when somebody is,
and which of the two you are in is measured rather than assumed.** A rule stated more
strongly than that gets switched off by the first person it interrupts, which leaves neither
a gate nor a warning.

The other half of that item — *an explicit note wherever nothing but discipline holds a rule
in place* — was always the stronger half, and it is already delivered: it is the four-state
section that every skill here carries, and the reason the middle state has its own name.

The second layer needs the first, which is why it is second. A knowledge base with several
writers and no rule about who holds the pen produces contradictory states that nobody
reports — and every rule in the knowledge layer is about keeping a fact answerable, which a
silent conflicting write defeats before any of them get a turn.

## Known limits

Each of these says what kind of claim it is — measured here, or merely carried forward — and
carries a date by which it should be looked at again. A named gap without a date stops being
a gap and turns into how the system simply is. The horizon is three months for all four:
Claude Code ships frequently enough that a longer one would be fiction, and often enough
that a shorter one would be busywork.

- **Whether plugin hooks run in claude.ai cloud sessions: unmeasured, and not documented
  anywhere we could find.** This release ships no hooks, so nothing here depends on the
  answer — but if you extend the plugin with a hook, do not assume it fires everywhere the
  skills do. *Re-check by 2026-12-13.*
- **Whether a plugin's skills and agents are available in the IDE integrations the same way
  they are in the terminal: unmeasured.** The plugin documentation does not mention IDE
  support either way, and we have not tested it. *Re-check by 2026-12-13.*
- **A plugin cannot ship a `CLAUDE.md`: measured 2026-09-13 on Claude Code 2.1.269.** A
  copy of this plugin was given a root `CLAUDE.md` containing a sentinel phrase and loaded
  with `--plugin-dir` from a directory with no project rules of its own. Asked to quote the
  instructions it had been given, the session reproduced the user-level rules at length,
  stated it had no project instructions, and never emitted the sentinel. So these rules take
  effect when a skill is invoked, not as ambient context in every session — which is what
  the opening section is about. *Measured through `--plugin-dir`; whether an installed
  plugin behaves identically here is not separately measured. Re-check by 2026-12-13.*
- **`hooks`, `mcpServers` and `permissionMode` in a plugin agent's frontmatter are silently
  ignored: carried over from the first release's notes, and the source for it was not
  re-located when this section was written.** It is why the agent relies on `tools:` and
  claims nothing else — a conservative choice that costs nothing even if the claim turns out
  to be wrong. "Silently" is the load-bearing word: there would be no error to notice either
  way, which is precisely why this one wants a measurement rather than a re-reading. The
  positive half *is* now measured: in a plugin agent's frontmatter, `tools:` and `model:`
  are both honoured — see the `foreign-material-reviewer` entry above, where the same run
  that showed the three-tool allowlist also showed the session running on the model the
  frontmatter names rather than the session default. So plugin agent frontmatter is read
  **selectively**, and which fields survive is a per-field question.
  *Measure the three ignored fields, or cite a source for them, by 2026-12-13.*

One further gap is named inside `delegation-contract` rather than here, because it is a
property of the rules and not of the packaging: a triage agent's *findings* flow back into
an agent that does hold Bash and write access, and nothing marks that return as foreign.
That seam is named, not closed.

## The pre-push hook

One rule here has a mechanical half, and this repository now runs it: nothing leaves this
repository except its git identity. `.githooks/pre-push` refuses a push whose commits carry
an identity this clone has not declared, and it refuses a push whose tracked content or
commit messages match a shape that personal data takes — an address of the kind mail uses, a
German phone number, an IBAN, a five-digit postcode followed by a place name. It reads every
commit in the pushed range rather than the tip alone, because a push publishes the whole
range, and a file removed in a later commit stays reachable by its hash for anyone who
clones. Commit messages are checked alongside the trees, since a message is as public as a
blob and trailers are where addresses ride in.

**It does not arrive with a clone.** Git runs hooks out of `.git/hooks` unless it is told
otherwise, and nothing in a checkout can tell it for you. Each clone needs one line:

```
git config core.hooksPath .githooks
```

A co-author or a fork declares a second identity with
`git config --add imprint.allowedIdentity 'Name <address>'`. Your own `user.name` and
`user.email` count as declared without being listed.

**What it does not enforce is the larger half.** The rule asks for abstraction: a worked case
told generically, with no organisation, no product, no ticket number, no path off anybody's
machine. Whether a passage is abstract is a question of meaning, and no pattern answers it. A
page naming a real employer in plain words passes this hook exactly as a properly abstracted
one does, and a blocklist of real names would not change that — it would only look as though
it had. In the four states this repository sorts every rule into: the shapes and the
identity are **enforced**; reading for abstraction stays a **behaviour rule** with nothing
behind it. The hook says so itself, in every report it prints.

It fails closed. Every way it can fail to finish — a git command that errors, an identity
this clone never set, a temporary directory it cannot create — ends in a refused push,
because a gate that waves you through when it breaks is indistinguishable from one that
checked. One empty case is not such a failure and took a refused push to find: git runs the
hook even when the remote is already up to date, and pipes in an empty ref list. Nothing is
published in that run, so there is nothing to check, and the hook says so and lets it
through — but only when git is the one calling, which is a hook invoked with a remote name
and location and handed a pipe rather than a terminal. An empty list from anything else is
still refused. A check that could not run is reported apart from a finding, and no override
covers it: "I could not look" and "I looked and found nothing" must never share an exit code.

`git push --no-verify` skips every hook silently, and the script cannot see that it happened.
`IMPRINT_PUSH_ANYWAY='reason' git push` is the loud alternative — the findings are printed in
full, the reason is echoed back, and the push proceeds.

## License

MIT, and it covers the whole work – the prose as much as any code. The MIT text speaks of
"the Software", which reads oddly for a repository that is mostly writing, so to be
explicit: the rules, the explanations and the examples are licensed on the same terms as
anything executable here. Copy them, adapt them, ship them inside your own systems.

## Contributing

Changes arrive as pull requests and are read and judged one at a time. A rule earns its
place by the failure it prevents, so the most useful thing a proposal can carry is the
case where the present text goes wrong. Corrections, counterexamples and "this does not
survive contact with my setup" are all welcome.
