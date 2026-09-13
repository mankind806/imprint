# imprint

A suite of working rules for two things that turn out to be one thing: how several AI
coding agents cooperate without ruining each other's work, and how a knowledge base grows
alongside the person using it without quietly rotting.

Both are written as mechanisms – what is enforced, what is only agreed, and how you tell
the two apart – rather than as advice.

It ships as a Claude Code plugin, so the rules arrive as skills the agent can reach for
during a session rather than as a document somebody has to remember to open.

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

The repository is its own marketplace, which is why the name appears twice; the
`@marketplace` suffix disambiguates, so plain `/plugin install imprint` also works while no
other marketplace you have added offers a plugin by that name. Once installed, the skills are
addressed as `/imprint:delegation-contract` and so on, and Claude reaches for them on its own
when a task matches their description.

## What is in the box

Three skills and one agent. Each skill states, for every rule it carries, what enforces it —
or says plainly that nothing does.

- **`delegation-contract`** – who leads and who advises, and how a delegated task tells the
  receiver which of the two it is. The header to put in front of a dispatch. Readers in
  parallel, exactly one writer at a time. Why a subagent's own success report does not count
  as evidence that it wrote anything, and why a required human yes does not travel with a
  delegation.

- **`blind-first-pass`** – how to set up a second opinion so it is worth having. Staged
  disclosure: the reproduction and the numbers first, your hypothesis last. When a closed
  question wants an independent *measurement* rather than another opinion. How to put
  disagreement in front of a person as a table of claims instead of averaging it away.

- **`measure-before-asserting`** – anything that can be executed, queried or looked up is,
  before it is said. Your own notes are a snapshot with a date on them, not a measurement.
  The failure mode is plausibility rather than ignorance, and an over-cautious false
  assurance is the dangerous kind because nothing ever makes it fail.

- **`foreign-material-reviewer`** (agent) – a read-only triage role for material you did not
  write. Its `tools:` frontmatter is an allowlist of `Read`, `Grep` and `Glob`, which is the
  point: a prompt asking an agent to stay read-only is a behaviour rule, and behaviour rules
  are broken by exactly the input this agent exists to handle.

## The two layers

**Agent collaboration is the foundation.** Who leads and who advises, and how a delegated
task tells the receiver which of the two it is. Readers in parallel, exactly one writer at
a time. When a second opinion adds information and when it only adds agreement – and why a
closed, checkable question wants an independent measurement rather than another opinion.
How to put disagreement in front of a human instead of averaging it away. Choosing the
cheapest model that clearly passes the job, and escalating to a stronger one instead of
resampling the same one. And ending a session as a handover, because the expensive thing to
reconstruct is not the unfinished file but the reason the next step was going to be that
step.

**The knowledge system sits on top.** One canonical place per fact, with every other view
generated, linked or embedded rather than copied. Provenance on every entry, including how
it was obtained. Superseding instead of deleting, so the replaced state stays provable
after the visible one changes. Gates that stop an action rather than warn about it, and an
explicit note wherever nothing but discipline holds a rule in place. And a standing
preference for measuring a property over citing your own notes about it, because notes are
a snapshot with a date on them, and a wrong one looks exactly like a right one.

The second layer needs the first. A knowledge base with several writers and no rule about
who holds the pen produces contradictory states that nobody reports.

## Status

Early, and deliberately partial.

**Only the first layer ships so far**, and only three of its rules. The knowledge-system
layer described above is intent, not content: there is nothing in this repository that
implements it yet. Read the section as a statement of where this is going, not as a
description of what you get.

The three skills that are here were practice before they were text, which is the right
order but means the text lags the practice and has not yet been read by anyone who did not
write it. Expect the structure to move before it settles.

## Known limits

Two of these are unmeasured rather than merely unfinished, and it seems better to say so
than to leave them out:

- **Whether plugin hooks run in claude.ai cloud sessions is not documented anywhere we
  could find, and we have not measured it.** This release ships no hooks, so nothing here
  depends on the answer — but if you extend the plugin with a hook, do not assume it fires
  everywhere the skills do.
- **IDE support is not mentioned in the plugin documentation.** We do not know whether
  skills and agents from a plugin are available in the IDE integrations the same way they
  are in the terminal. Untested.

Two further limits are known rather than unmeasured. A plugin cannot ship a `CLAUDE.md`: a
rules file placed in the plugin root is not loaded, so these rules take effect when a skill
is invoked, not as ambient context in every session. And `hooks`, `mcpServers` and
`permissionMode` in a plugin agent's frontmatter are silently ignored, which is why the
agent here relies on `tools:` and claims nothing else.

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
