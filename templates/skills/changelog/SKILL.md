---
name: changelog
description: Update CHANGELOG.md before merging a phase. Use at the end of a Spec-Driven Development phase, after the work is validated and before the branch merges.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

# changelog: record what shipped before a merge

You are closing a phase. Update `CHANGELOG.md` so the history stays legible without reading the diff.

## Steps

1. Read `CHANGELOG.md` and the current phase's `specs/YYYY-MM-DD-<feature>/requirements.md`.
2. Confirm the work is validated against that phase's `validation.md`. If it is not, stop and say what is unproven. Do not write a changelog entry for unvalidated work.
3. Add an entry under an `## [Unreleased]` heading (create it if absent), grouping lines under `Added`, `Changed`, `Fixed`, or `Removed`. Write each line in terms a reader outside the work would understand, describing the outcome, not the implementation.
4. Keep it short: what changed and why it matters, not a commit-by-commit replay.

## Format

```
# Changelog

## [Unreleased]

### Added
- <capability now available, in owner-facing terms>

### Changed
- <behaviour that is different now, and why>

### Fixed
- <what was broken and now works>
```

After writing the entry, the phase can merge. Then set the phase to `complete` in `specs/roadmap.md` with a dated line citing the validation evidence.
