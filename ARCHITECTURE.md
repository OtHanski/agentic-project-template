<!--
  TEMPLATE — owned by ARCH.00 (see CLAUDE.md). This is a *living* document: update it
  as design decisions are made, not just at the start. Delete this instruction block
  once the sections below have real content.
-->

# {{PROJECT_NAME}} Architecture

## Overview

{{2-4 sentences: what the system does, and the shape of it — e.g. "a client-server pair
that talks over X", "a single binary with pluggable backends", "a library plus one thin
CLI wrapper".}}

## Components

| Component      | Owner   | Responsibility                          |
|----------------|---------|------------------------------------------|
| {{name}}       | {{ID}}  | {{one line}}                              |

## Approved External Dependencies

Record *every* external dependency here with why it was approved, even "none" is a
decision worth writing down. Keeping this list explicit and reviewed prevents dependency
creep task-by-task, where each individual addition looks reasonable but the aggregate
isn't.

| Dependency | Version/pin | Used by      | Why approved                        |
|------------|-------------|--------------|--------------------------------------|
| {{name}}   | {{version}} | {{component}}| {{reason — what it would cost to hand-roll instead}} |

## Interface Contracts

For each boundary between components, the contract both sides are held to. This is
the section ARCH.00 writes *before* filing implementation tasks for a feature that
crosses a boundary — see CLAUDE.md's Coordination Workflow, Step 1.

### {{Component A}} ↔ {{Component B}}

{{What crosses this boundary, in what shape, who may change it unilaterally (usually:
no one — see docs/SPEC.md if there's a versioned spec document for this boundary).}}

## Design Decisions Log

Append-only. When a design conflict between agents' outputs is resolved, or a
non-obvious tradeoff is chosen, record it here with the date and the task that prompted
it, so a later agent doesn't silently re-litigate or reverse it without knowing why.

- **{{YYYY-MM-DD}}** — {{decision}}. **Why:** {{reasoning}}. (`{{TASK-NNNNN}}`)

## Implementation Phases

Kept current as milestones close — see CLAUDE.md's Coordination Workflow, Step 8. A
phase row is added or corrected in the *same change* that closes the milestone, not
backfilled later.

| Phase | Feature/Milestone | Status  | Tasks           | Closed     |
|-------|-------------------|---------|------------------|------------|
| 1     | {{name}}          | done    | {{TASK-00001}}–{{TASK-00012}} | {{YYYY-MM-DD}} |
