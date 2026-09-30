# Multi-Agent Project Template

This is a starting kit for a new software project that will be built primarily by an AI
coding agent (or a small team of agents) under human direction, using a persistent
`TODO/`-based task system and a small set of living documents instead of ad-hoc chat
history as the source of truth.

It is extracted from a real project's `CLAUDE.md` after that workflow had been running
long enough to show what actually mattered. It is deliberately **not** tied to any
language, platform, or problem domain — the mechanics (fixed agent IDs, folder-as-stage
task files, spec-before-implementation for shared contracts, review gating) are the part
worth reusing; the specific roles and docs are examples to replace.

## What's in here

```
CLAUDE.md              — agent roster + coordination protocol (the core of this template)
ARCHITECTURE.md         — skeleton for the living system-design doc
docs/
  PHILOSOPHY.md         — engineering principles, mostly reusable as-is
  STYLE.md              — skeleton for project-wide style decisions
  SPEC.md               — skeleton for a shared contract doc (protocol/API/schema/etc.)
  RELEASE.md            — skeleton for build/release workflow
TODO/
  TEMPLATE.md           — the task file template
  todo/  review/  done/ — the three task-stage folders (start empty)
```

## If you are an agent bootstrapping a new project from this template

Do the following, in order. Don't just fill in placeholders mechanically — the roster,
docs, and even the workflow steps should be reshaped to fit the actual project, not
forced into VaporWault's original shape (three-component C/C++ app). What must survive
is the *mechanics* in the "Load-bearing mechanics" section below, not any specific
wording.

1. **Understand the project first.** If the user hasn't already told you, ask (don't
   guess): what is being built, in what language(s)/platform(s), what are its natural
   module boundaries (the seams along which one person/agent could own a piece without
   constantly stepping on another), what's the external-dependency policy, and roughly
   how much concurrent agent activity is expected (a solo hobby project run by one agent
   at a time needs a much smaller roster than a project running several agents in
   parallel).

2. **Design the agent roster.** Copy the pattern in `CLAUDE.md`'s Agent Roster table:
   fixed short IDs (`ARCH.00` plus `XXX.NN` for each domain), one clear domain per
   agent, a "reads/writes" contract for how each agent touches the TODO system. Keep
   `ARCH.00` (architect/orchestrator) — every project needs one coordinating role even
   if it's small. Size the rest to the project: a small project might only need
   `ARCH.00` + one implementer + one reviewer; don't invent roles (e.g. separate mobile
   or web-gateway roles) the project doesn't actually have a component for.

3. **Fill in `CLAUDE.md`.** Replace the placeholder project description, roster, and
   persistent-documents table. Keep the TODO-List Protocol and Coordination Workflow
   sections' *structure* — they're already domain-agnostic — but adjust step names/roles
   to match your roster (e.g. if there's no separate protocol-spec step, drop it; if
   there's no GUI-consumes-library boundary, drop that constraint).

4. **Fill in `ARCHITECTURE.md`.** At minimum: component overview, approved external
   dependencies (even "none" is a decision worth recording), and an empty Implementation
   Phases table ready to be filled in as milestones close.

5. **Review `docs/PHILOSOPHY.md`.** It's written to be reusable across most projects.
   Trim or extend it for domain-specific concerns (e.g. add a memory-safety section for
   a systems-language project, or a data-privacy section for anything handling user
   data), but don't delete it wholesale — it's what keeps agents from over-engineering.

6. **Fill in `docs/STYLE.md` and `docs/RELEASE.md`** only as far as you can commit to
   right now (a couple of real decisions, not a wishlist) — both are meant to accumulate
   decisions over time, not be complete on day one.

7. **Decide whether you need `docs/SPEC.md`.** This is for a shared contract that more
   than one component must independently agree on and can't renegotiate ad hoc — a wire
   protocol, a public API, a database/file-format schema, an IPC message format. If
   nothing in the project has that shape, delete this file. If something does, name one
   agent as its owner and route to it exactly like a protocol doc: consumers block on a
   published spec section before implementing, and they never unilaterally reinterpret
   an ambiguity — they flag it back to the owner.

8. **Set up `TODO/`.** Keep `TEMPLATE.md` and the three empty folders. Write the first
   handful of real tasks (the initial feature breakdown) directly — don't leave the
   system empty for the user to bootstrap themselves.

9. **Initialize git** (if not already) and commit the filled-in template as the first
   commit, so the coordination system is in place before any feature work starts.

## Load-bearing mechanics (keep these no matter how much else you reshape)

These are the specific things that, in practice, kept the original project's task system
trustworthy instead of drifting into noise. Each came from a concrete failure mode —
worth understanding why before you're tempted to simplify it away:

- **Fixed, stable agent IDs** used everywhere (task `assignee`/`review_by` fields, code
  review comments, architecture notes). Free-text names drift and get typo'd; a fixed ID
  is greppable and unambiguous about who's responsible.
- **A task's folder (`todo/` / `review/` / `done/`) always matches its `status:` field,
  and both change in the same commit.** The folder tracks *stage* (pre-review /
  in-review / closed); `status:` tracks the precise state within that stage
  (`blocked` doesn't get its own folder — it stays wherever it got stuck). If you ever
  find a mismatch, that's a bug to fix immediately, not a style nit — agents and humans
  both use folder location as a trustworthy filter ("what's actually in flight"), and it
  silently stops being trustworthy the moment status and location can disagree.
- **Quote YAML title fields.** An unquoted scalar with `: ` in it is ambiguous between a
  plain string and a nested mapping across parsers — quote every `title:` unconditionally
  rather than rely on remembering which titles happen to contain a colon.
- **Out-of-domain discovery creates a new task for the right owner; it does not get
  fixed inline.** Otherwise ownership boundaries erode task by task and the roster stops
  reflecting who actually touches what.
- **A reviewer's `blocking` finding is a hard gate on `done`,** separate from
  `advisory` suggestions that don't block. Without that distinction, review either
  rubber-stamps everything or blocks on everything.
- **Security-sensitive (or equivalent high-risk-domain) tasks always list every
  required reviewer** — don't let a task slip through review-light just because someone
  forgot to tag it; make the tag imply the reviewer list, not the other way around.
- **Shared-contract changes (protocol/API/schema) get a published spec update before
  dependent implementation starts**, not implemented first and documented after the fact.
  Divergent client/server (or equivalent) implementations against an undocumented,
  half-remembered contract is the failure mode this prevents.
- **Cross-cutting consumer tasks (a UI that must catch up to a backend change, a mobile
  client that must catch up to a protocol change, etc.) are filed in the same breakdown
  as the feature that creates the need**, not left to be noticed reactively later. This
  project's own history shows the reactive version: a full-project review found a whole
  downstream role sitting behind for an extended period, discovered only by manual audit.
- **A living architecture/design doc's summary tables (phases, milestones, whatever the
  project's equivalent is) get updated in the same change that closes the milestone**,
  not backfilled later. This drifted stale three separate times in the source project
  before the rule was written down explicitly enough for an agent to actually follow it
  — "update the doc eventually" without a a forcing function this precise (in *this*
  change) doesn't work.
- **Only the orchestrator role deletes or reorders tasks.** Everyone else appends and
  updates status. This keeps task history an append-only audit trail instead of
  something that can be quietly rewritten.
