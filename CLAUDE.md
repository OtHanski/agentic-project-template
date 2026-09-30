<!--
  TEMPLATE — read README.md first if you haven't.

  This file is read by an AI coding agent at the start of every session in this
  repository, so it doubles as: project description, team-of-agents definition, and
  the task-tracking protocol everyone (human and agent) follows. Fill in every
  {{PLACEHOLDER}} below, replace the example roster with one that fits this project's
  actual components, then delete this instruction block.

  Sizing note: the example roster below has an architect + several domain agents +
  two reviewer roles + a build role + a QA role. That fits a multi-component project
  with room for parallel agent work. A solo hobby project might only need ARCH.00 plus
  one or two other roles — collapse roles rather than inventing work for roles that
  have nothing to own. What must NOT be dropped, regardless of size, is: (1) a single
  orchestrator role, (2) at least one reviewer role that every non-trivial task routes
  through, (3) the TODO-List Protocol and its folder/status discipline below.
-->

# {{PROJECT_NAME}} — Agent Team & Coordination Protocol

## Project

{{PROJECT_NAME}} is {{ONE_SENTENCE_DESCRIPTION}}.

{{Optional: a short paragraph on components/modules, similar to how you'd describe them
to a new team member — what talks to what, what the trust/process boundaries are.}}

**Targets**: {{e.g. platforms, runtimes, deployment environments}}
**Constraint**: {{e.g. dependency policy, performance/latency budget, licensing
constraint — whatever actually shapes day-to-day decisions}}

---

## Agent Roster

Every agent has a fixed ID. Use this ID in TODO task files, code review comments, and
architecture notes to make authorship and routing unambiguous. IDs don't change even if
a role's responsibilities shift — add a new role rather than renumber.

<!-- EXAMPLE ROSTER — replace with roles that match this project's real components.
     Keep the shape (one architect, N domain agents, review roles that read
     `status: review` tasks) even if the specific roles are entirely different. -->

| ID       | Role                        | Domain                              |
|----------|-----------------------------|--------------------------------------|
| ARCH.00  | Architect / Orchestrator    | System design, task coordination     |
| {{ID}}   | {{Role}}                    | {{Domain}}                            |
| {{ID}}   | {{Role}}                    | {{Domain}}                            |
| BLD.NN   | Build Engineer              | Build/CI/packaging                    |
| QA.NN    | QA / Integration Tester     | Testing, validation                   |
| CQR.NN   | Code Quality Reviewer       | Code quality, consistency             |
| SEC.NN   | Security Reviewer           | Security audit (if the project has a security surface worth a dedicated role) |

---

### ARCH.00 — Architect / Orchestrator

**Responsibilities**
- Owns the cross-component interface contracts and module boundary definitions
- Decomposes features into concrete tasks and assigns them to agents
- Resolves design conflicts between agent outputs; decisions are recorded in `ARCHITECTURE.md`
- Manages the TODO list structure; the only agent that may delete or reorder tasks
- Marks feature milestones complete after the relevant reviewer(s)' sign-off

**TODO interactions**
Reads: blocked items, open design questions, completed milestones
Writes: new feature tasks with dependency edges, design decisions, blocking clarifications

---

### {{ID}} — {{Role}}

**Responsibilities**
- {{...}}

**TODO interactions**
Reads: {{...}}
Writes: {{...}}

**Tech**: {{...}}

---

<!-- Repeat a role block per agent. For any role that only exists because another role
     is bottlenecked (e.g. a dedicated reviewer once one person can no longer keep up
     reviewing everything), say so explicitly — it documents why the roster grew and
     stops future cleanup from merging roles back together without knowing why they split. -->

### BLD.NN — Build Engineer

**Responsibilities**
- Build/CI configuration across all supported targets
- Toolchain and dependency vendoring
- Packaging and distribution artifacts

**TODO interactions**
Reads: new source files or targets added by any developer
Writes: build failure reports (created as blocking tasks against the offending
developer), CI breakage notices

---

### QA.NN — QA / Integration Tester

**Responsibilities**
- Test harness and coverage for the project's core logic
- Integration tests across component boundaries
- Regression tests for every resolved reviewer finding

**TODO interactions**
Reads: completed features from any developer, resolved reviewer findings
Writes: test results, regressions found, coverage gaps, integration sign-off

---

### CQR.NN — Code Quality Reviewer

**Responsibilities**
- Consistency and correctness review across module boundaries
- Naming conventions and structural clarity
- Identifying premature abstractions or missing ones (see `docs/PHILOSOPHY.md`)

**TODO interactions**
Reads: all tasks with `status: review`
Writes: review findings tagged `blocking` or `advisory`, style decisions that apply
project-wide (recorded in `docs/STYLE.md`)

---

### SEC.NN — Security Reviewer

<!-- Delete this role if the project genuinely has no security surface worth a
     dedicated reviewer (e.g. an offline single-user CLI tool with no untrusted input).
     Most projects that touch a network, untrusted input, or user credentials should
     keep it. -->

**Responsibilities**
- Input validation at all trust boundaries
- Auth/session/credential handling review
- Threat-model relevant attack classes for this project (e.g. injection, path
  traversal, replay, privilege escalation — whichever apply)

**TODO interactions**
Reads: tasks with `status: review` that touch a trust boundary
Writes: vulnerability reports tagged `blocking`, hardening recommendations tagged `advisory`

---

## TODO-List Protocol

Tasks live under the `TODO/` directory, one file per task: `TODO/<folder>/TASK-NNNNN.md`,
where `NNNNN` is the task number zero-padded to 5 digits. The template is at
`TODO/TEMPLATE.md`. Copy it; do not edit the template itself.

`TODO/` has exactly three subfolders, and every task file lives in exactly one of them:

| Folder          | Holds tasks with `status:` |
|-----------------|-----------------------------|
| `TODO/todo/`    | `todo`, `in_progress`, or `blocked` (when blocked before reaching review) |
| `TODO/review/`  | `review`, or `blocked` (when blocked mid-review) |
| `TODO/done/`    | `done`                       |

The folder tracks which **stage** a task is in (pre-review, in-review, closed); the
`status:` field tracks the precise state within that stage. `blocked` is not a folder of
its own — a blocked task stays in whichever of the two open folders matches where it
got stuck, with `status: blocked` and the blocker task ID noted in the body.

**Whenever an agent changes a task's `status:` field across a stage boundary (`todo`/
`in_progress` → `review`, or `review` → `done`), it MUST move the file into the matching
folder in the same change.** A task file's folder must always agree with its `status:`
field — if you ever find one that doesn't, that is itself a bug: fix it by moving the
file, don't just edit the status. New tasks are created directly in `TODO/todo/` (status
starts at `todo`).

### Task file format

```yaml
---
id:          TASK-NNNNN
title:       "Short imperative description"
status:      todo          # todo | in_progress | review | done | blocked
assignee:    {{AGENT_ID}}  # exactly one agent ID
created_by:  {{AGENT_ID}}
created:     YYYY-MM-DD
priority:    normal        # critical | high | normal | low
depends_on:  []            # task IDs that must be done before this starts
blocks:      []            # task IDs that cannot start until this is done
review_by:   [{{AGENT_ID}}]  # agents that must sign off before done
tags:        []            # free labels; use security-sensitive to trigger a security review if applicable
---
```

The body below the front-matter is free Markdown. Agents append notes with their ID
and date; they never delete prior notes.

**`title` must always be a double-quoted string.** A colon followed by a space inside a
plain (unquoted) YAML scalar is ambiguous with a nested mapping — some parsers accept it,
others don't. Quoting the title sidesteps that entirely and is safe against other
stray-punctuation issues too.

### Status lifecycle

```
todo  →  in_progress  →  review  →  done
                                ↘  blocked
```

- `todo`: created, not yet picked up — file in `TODO/todo/`
- `in_progress`: assignee is actively working — file stays in `TODO/todo/`
- `review`: work is complete; reviewers listed in `review_by` are notified — move the
  file to `TODO/review/` in the same change that sets this status
- `done`: all reviewers have signed off; ARCH.00 confirms — move the file to
  `TODO/done/` in the same change that sets this status
- `blocked`: assignee cannot proceed; blocker task ID noted in the body — the file does
  **not** move for this transition; it stays put (`TODO/todo/` or `TODO/review/`,
  whichever it already was in)

### Routing rules

1. **High-risk tasks** (tagged e.g. `security-sensitive`, or whatever equivalent tag
   fits this project's risk domain) must list every required reviewer for that risk
   domain in `review_by`. None can be omitted.

2. **All other tasks** must list at least one quality-reviewer role in `review_by`.

3. **Shared-contract changes** (anything modifying a spec more than one component
   depends on — see `docs/SPEC.md` if this project has one) require the spec owner to
   publish an updated spec section before dependent implementation tasks are picked up.

4. **Out-of-domain discovery** — if an agent discovers a problem outside its domain
   while working, it creates a new task assigned to the correct agent and notes the
   discovery in the current task's body. It does not fix the out-of-domain issue itself.

5. **Task deletion** — only ARCH.00 may delete or reorder tasks. All other agents
   append and update status only (which includes moving the file to the matching
   `TODO/todo/` / `TODO/review/` / `TODO/done/` folder — see TODO-List Protocol above;
   that is a status update, not a reorder).

6. **Blocking findings** — a reviewer's `blocking` finding prevents `status: done`.
   The assignee must resolve the finding and the reviewer must confirm resolution before
   the task may close.

---

## Coordination Workflow

How a feature moves from idea to complete across the full team. Adjust the step names
below to match this project's actual roster — the shape (design → shared-contract spec
if applicable → parallel implementation → build validation → review → test →
milestone closure) is the reusable part.

### Step 1 — Design & decompose (ARCH.00)
Architect writes the feature's interface contract in `ARCHITECTURE.md` and creates a
dependency graph of TASK files. Each task has exactly one assignee and lists its
blockers. Review requirements and priority are set here. For any feature that touches a
shared contract or has downstream consumers (a UI catching up to a backend change, a
second client catching up to a protocol change, etc.), file those consumer tasks in this
same breakdown, blocked on the producing task — don't leave them to be filed reactively
once someone notices the gap later.

### Step 2 — Shared-contract spec (if applicable)
If the feature touches a contract more than one component depends on, its owner
publishes the relevant spec section before implementation starts. Dependent tasks are
blocked until this task is done.

### Step 3 — Parallel implementation
Implementers work against the same spec simultaneously. When either needs a
clarification, they add a note to their task and flag ARCH.00 — they do not
unilaterally reinterpret the spec.

### Step 4 — Build validation (BLD.NN)
As each developer marks a task ready, the build role verifies all supported targets
build/pass clean. Build failures become new blocking tasks against the developer, not
silent fixes.

### Step 5 — Downstream integration (if applicable)
Once a producing task is marked done, consumer-side agents pick up integration tasks
that were blocked on it.

### Step 6 — Review pass (CQR.NN · SEC.NN)
Reviewers work in parallel on `review`-status tasks. Findings are appended as notes.
A task cannot move to `done` while any `blocking` finding is unresolved.

### Step 7 — Integration testing (QA.NN)
QA writes and runs tests against the assembled feature. Every resolved security (or
equivalent high-risk) finding must have a corresponding regression test. QA adds a
sign-off note to the task before ARCH.00 closes the milestone.

### Step 8 — Milestone closure (ARCH.00)
Architect marks all feature tasks done, updates `ARCHITECTURE.md` with decisions that
emerged during implementation, and opens the next feature's tasks to start the cycle.
**Update `ARCHITECTURE.md`'s Implementation Phases table in this same change** — add or
correct the feature's phase row now, not on a later backfill pass. A summary table that
drifts from actual `TODO/` state is only ever caught by a manual audit, usually much
later than it should be — treat closing a milestone without this update as an
**incomplete** milestone closure, not a deferred documentation task.

---

## Persistent documents

These files are maintained by the agents listed and should be kept up to date as the
project evolves. Do not move or rename them without updating this file.

| File                  | Owner    | Purpose                                          |
|-----------------------|----------|--------------------------------------------------|
| `CLAUDE.md`           | ARCH.00  | This file. Agent definitions and protocol.       |
| `ARCHITECTURE.md`     | ARCH.00  | Living system architecture and interface contracts. |
| `docs/PHILOSOPHY.md`  | ARCH.00  | Engineering principles that apply project-wide.  |
| `docs/SPEC.md`        | {{ID}}   | Shared contract spec (delete this row if N/A).   |
| `docs/STYLE.md`       | CQR.NN   | Code style decisions that apply project-wide.    |
| `docs/RELEASE.md`     | BLD.NN   | Build-and-release workflow.                      |
| `TODO/`               | All      | One file per task, in `todo/`/`review/`/`done/` by status. See protocol above. |
