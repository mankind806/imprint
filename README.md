# imprint

A suite of working rules for two things that turn out to be one thing: how several AI
coding agents cooperate without ruining each other's work, and how a knowledge base grows
alongside the person using it without quietly rotting.

Both are written as mechanisms rather than as advice, and every rule says which of three
things it is: enforced by something that stops you, *enforceable* but currently resting on
discipline, or a behaviour rule with nothing behind it at all.

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

**Only the first layer ships so far**, and only three of its rules. The knowledge-system
layer sketched below is intent, not content: there is nothing in this repository that
implements it. Read that section as a statement of direction, not as a description of what
you get.

The three skills that are here were practice before they were text, which is the right
order but means the text lags the practice. It has now had one adversarial read by a party
that did not write it; expect the structure to move again before it settles.

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
--plugin-dir … -p` lists all three skills and the agent under the `imprint:` prefix.* Two
things about that measurement are worth stating rather than glossing:

- It exercised a **local path**, not the `mankind806/imprint` shorthand, which adds a clone
  step before the same resolution. That shorthand has since been measured on its own —
  2026-09-13, Claude Code 2.1.269, after the first push, from an isolated configuration.
  `marketplace add mankind806/imprint` cloned over HTTPS and validated, the qualified
  `install` reported success, and a session started afterwards listed all three skills and
  the agent, loaded out of the installed plugin cache rather than out of any checkout. So a
  marketplace whose plugin `source` is the repository root does resolve over the network;
  had it not, the failure would have been at install time and total.
- Whether the bare `install imprint` stays unambiguous depends on the other marketplaces
  *you* have added. It was measured with no competing plugin of that name present, which is
  the only condition under which the shorthand is meaningful at all. If you already have an
  `imprint` from somewhere else, use the qualified form.

## What is in the box

Three skills and one agent. Each skill carries a section on what actually enforces it, and
sorts every rule it holds into one of three states: **enforced** by something that really
stops you, **enforceable but not enforced** where a mechanism is possible and nobody has
built it, and a plain **behaviour rule** that holds only as long as the discipline does. The middle state is the one usually left out, and
it is the useful one — it is a list of the places where a few lines of tooling would pay.

- **`delegation-contract`** – who leads and who advises, and how a delegated task tells the
  receiver which of the two it is – including why the flag you were invoked with does not
  answer that question. The header to put in front of a dispatch. Readers in parallel,
  exactly one writer at a time, and where "do not do it yourself" stops. Why a subagent's
  own success report does not count as evidence that it wrote anything, and why a required
  human yes does not travel with a delegation.

- **`blind-first-pass`** – how to set up a second opinion so it is worth having. Staged
  disclosure: the reproduction and the numbers first, your hypothesis last. Why a
  context-inheriting dispatch silently gives you the opposite of a blind review. When a
  closed question wants an independent *measurement* rather than another opinion. How to put
  disagreement in front of a person as a table of claims instead of averaging it away.

- **`measure-before-asserting`** – anything that can be executed, queried or looked up is,
  before it is said. Your own notes are a snapshot with a date on them, not a measurement.
  The failure mode is plausibility rather than ignorance, and an over-cautious false
  assurance is the dangerous kind because nothing ever makes it fail.

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

**Agent collaboration is the foundation, and it is what ships.** Who leads and who advises,
and how a delegated task tells the receiver which of the two it is. Readers in parallel,
exactly one writer at a time. When a second opinion adds information and when it only adds
agreement – and why a closed, checkable question wants an independent measurement rather
than another opinion. How to put disagreement in front of a human instead of averaging it
away. Escalating to a stronger voice rather than resampling the same one after it has
already failed twice.

**A knowledge system is meant to sit on top. None of it is written yet.** The intended
shape: one canonical place per fact, with every other view generated, linked or embedded
rather than copied. Provenance on every entry, including how it was obtained. Superseding
instead of deleting, so the replaced state stays provable after the visible one changes.
Gates that stop an action rather than warn about it, and an explicit note wherever nothing
but discipline holds a rule in place. And a standing preference for measuring a property
over citing your own notes about it — the one part of that list which *has* been written,
as `measure-before-asserting`.

The second layer will need the first. A knowledge base with several writers and no rule
about who holds the pen produces contradictory states that nobody reports.

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
