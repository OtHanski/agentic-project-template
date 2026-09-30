<!--
  This one is written to be reusable across most projects with little editing — it's
  general engineering judgment, not project specifics. Trim what doesn't apply, extend
  it for domain-specific concerns (memory safety for a systems-language project, data
  privacy for anything handling user data, etc.), but don't delete it wholesale: it's
  what keeps agents from over-engineering or under-engineering by default.
-->

# Engineering Philosophy

Principles every agent on this project follows, regardless of role. Reviewers
(CQR-equivalent role) should treat violations of these as findings, not just style taste.

## Scope discipline

- Don't add features, refactor, or introduce abstractions beyond what the task requires.
  A bug fix doesn't need surrounding cleanup; a one-shot operation doesn't need a helper.
- Don't design for hypothetical future requirements. Three similar lines of code is
  better than a premature abstraction built to anticipate a fourth that may never come.
- Don't add error handling, fallbacks, or validation for scenarios that can't actually
  happen given the surrounding guarantees. Trust internal code and framework invariants.
  Validate only at real system boundaries: user input, external APIs, untrusted network
  input, filesystem/OS interaction.
- No half-finished implementations. If a task is too large to do properly in one pass,
  say so and split it into tracked tasks rather than landing something partial silently.
- Avoid backwards-compatibility shims, feature flags, or "just in case" fallback paths
  for internal code you can simply change outright. This project doesn't have external
  API consumers to protect until it explicitly does (see `ARCHITECTURE.md`'s Interface
  Contracts section for boundaries that *do* need that discipline).

## Code shape

- Default to writing no comments. Add one only when the *why* is non-obvious: a hidden
  constraint, a subtle invariant, a workaround for a specific bug, behavior that would
  surprise a careful reader. If removing the comment wouldn't confuse a future reader,
  don't write it.
- Never explain *what* code does in a comment — a well-named identifier already does
  that. Never reference the current task, a bug number, or "added for X flow" in a
  comment; that belongs in the task file or commit message, and it rots as the code
  evolves out from under it while the comment stays.
- Naming and structure should make intent legible without a guide. If a reviewer has to
  ask what something is for, that's a naming problem to fix, not a comment to add.
- Prefer editing existing files/modules to creating new ones when the change is a
  natural extension of what's already there.

## Correctness and safety

- Be careful not to introduce injection, overflow, path-traversal, or other
  boundary-crossing vulnerabilities. If you notice you've written something unsafe, fix
  it immediately rather than filing it as a follow-up — unlike scope creep, safety
  defects don't get better by waiting.
- Type/static checking and test suites verify code correctness, not feature
  correctness. If a change is user-facing and you can exercise it directly (run the
  app, hit the endpoint, drive the UI), do that before calling the task done. If you
  can't exercise it in this environment, say so explicitly rather than implying you did.
- Every resolved security or correctness finding from a reviewer gets a regression
  test, not just a fix — see CLAUDE.md's Coordination Workflow, Step 7.

## Reversibility and blast radius

Before taking an action, consider how hard it would be to undo and how far its effects
reach beyond the local, in-progress change:

- Freely proceed with local, reversible actions: editing files, running local tests,
  local builds.
- Pause and confirm with the human (or, in a multi-agent setup, escalate to ARCH.00)
  before anything hard to reverse or externally visible: force-pushing, resetting
  shared branches, deleting data, downgrading/removing dependencies, modifying shared
  infrastructure or CI configuration, publishing/releasing artifacts, sending messages
  or opening/closing issues and PRs on someone else's behalf.
- When you hit an obstacle, find the root cause rather than reaching for a destructive
  shortcut (skipping a check, force-overwriting, disabling a test) to make it go away.
  If you discover unfamiliar in-progress state (uncommitted changes, an unfamiliar
  branch), investigate before touching it — it may be someone else's work.

## Task hygiene

- Out-of-domain discoveries get filed as a new task for the right owner, not fixed
  inline (see CLAUDE.md's routing rules) — this is a scope-discipline rule as much as a
  process one: fixing it yourself means you're now reviewing your own out-of-domain
  change with none of the context the right owner would have brought.
- A task isn't done because the code works; it's done when it's met its `review_by`
  bar. Don't mark `done` preemptively to unblock a dependent task — block the dependent
  task instead, honestly.
