<!--
  Copy this file to TODO/todo/TASK-NNNNN.md when creating a new task — do not edit this
  template in place. NNNNN is the next task number, zero-padded to 5 digits. New tasks
  always start in TODO/todo/ with status: todo.

  See CLAUDE.md's TODO-List Protocol for the full rules (folder/status agreement,
  routing rules, status lifecycle). The two things most commonly gotten wrong:
    - title must be a double-quoted string, always (see CLAUDE.md for why)
    - when status crosses a stage boundary, move the file in the same change
-->
---
id:          TASK-NNNNN
title:       "Short imperative description"
status:      todo          # todo | in_progress | review | done | blocked
assignee:    {{AGENT_ID}}   # exactly one agent ID
created_by:  {{AGENT_ID}}
created:     YYYY-MM-DD
priority:    normal         # critical | high | normal | low
depends_on:  []             # task IDs that must be done before this starts
blocks:      []             # task IDs that cannot start until this is done
review_by:   [{{AGENT_ID}}] # agents that must sign off before done
tags:        []             # free labels; use e.g. security-sensitive to trigger required reviewers
---

## Description

{{What needs to happen, and why — enough context that the assignee doesn't need to
re-derive intent from the task title alone.}}

## Acceptance criteria

- {{Concrete, checkable condition for "done."}}

## Notes

<!-- Agents append dated notes below with their ID. Never delete prior notes. -->
