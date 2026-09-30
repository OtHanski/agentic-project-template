<!--
  TEMPLATE — owned by the build-engineer role (see CLAUDE.md). Fill in as far as you can
  commit to now; this is expected to grow as the release process matures. Delete this
  instruction block once it has real content.
-->

# {{PROJECT_NAME}} Build & Release Workflow

## Build

{{How to build from a clean checkout, for every supported target. Exact commands, not
a description — an agent should be able to follow this section literally.}}

## Versioning

{{Scheme used (semver, calendar, etc.) and where the version number lives — one source
of truth, not duplicated across files.}}

## Release artifacts

{{What gets produced, where it's published, who/what triggers it (manual step vs. CI).}}

## Dry-run / verification

{{The check performed before a release is considered good — smoke test, checklist,
whatever's real. If there isn't one yet, say so explicitly rather than leaving this
section silently empty.}}
