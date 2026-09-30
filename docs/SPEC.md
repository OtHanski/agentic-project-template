<!--
  TEMPLATE — delete this entire file if the project has no shared contract that
  multiple independently-owned components must agree on. Keep it only if something like
  a wire protocol, a public API, a database/file-format schema, or an IPC message format
  exists and more than one agent's component depends on it.

  If you keep it: name exactly one owner role in CLAUDE.md's roster and its persistent
  documents table. That owner publishes sections here *before* dependent implementation
  tasks are picked up (see CLAUDE.md's Coordination Workflow, Step 2). Consumers never
  unilaterally reinterpret an ambiguous section — they flag it back to the owner and
  wait for a published update, the same way SRV.01/CLI.02 do for PRT.04's protocol
  spec in the source project this template comes from.
-->

# {{PROJECT_NAME}} Shared Contract Spec

Owner: {{ID}}

## Versioning

{{How this spec is versioned, and how a breaking change is signaled to consumers —
e.g. a version field in every message, a changelog section per release.}}

## Version History

| Version | Date       | Summary of change      | Task        |
|---------|------------|--------------------------|-------------|
| {{v1}}  | {{YYYY-MM-DD}} | {{initial spec}}      | {{TASK-NNNNN}} |

## {{Section per message/endpoint/table/whatever the contract's unit is}}

{{Full definition: shape, fields, semantics, error cases. Precise enough that two
independent implementations against this section alone would interoperate.}}
