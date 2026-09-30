<!--
  TEMPLATE — owned by the code-quality-reviewer role (see CLAUDE.md). This is meant to
  accumulate real decisions over time, made when a reviewer finding generalizes beyond
  the one task it was raised on — not to be filled out speculatively on day one. Seed it
  with only the handful of decisions you can actually commit to now; delete this
  instruction block once it has real content.
-->

# {{PROJECT_NAME}} Style Decisions

Project-wide style and consistency decisions. Each entry exists because a reviewer
finding generalized beyond the single task it was raised on — this file is where that
generalization gets written down so the next task doesn't re-raise it as a fresh finding.

Format: one entry per decision, dated, with which task prompted it and (briefly) why.

## Language / formatting

- {{e.g. formatter + config used, and whether it's enforced in CI}}

## Naming conventions

- {{e.g. casing rules per identifier kind, module/file naming}}

## Error handling conventions

- {{e.g. how errors propagate — exceptions vs. return codes vs. Result types — and where
  each is expected}}

## Decisions log

- **{{YYYY-MM-DD}}** ({{TASK-NNNNN}}) — {{decision}}. **Why:** {{reasoning}}.
