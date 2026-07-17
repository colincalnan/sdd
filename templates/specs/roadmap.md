---
title: "{{project_title}}: Roadmap"
type: constitution
updated: {{date}}
---

# Roadmap

*Part of the constitution and the living source of truth for what is next. The agent reads this first every session and works the top phase that is not complete. Each phase is independently shippable: buildable, testable, and mergeable on its own, with no dependency on a future phase being started.*

Status values: `pending`, `in-progress`, `complete`.

## Phases

| # | Phase | Status | Spec folder | Notes |
|---|---|---|---|---|
| 1 | {{phase_1}} | in-progress | | |
| 2 | {{phase_2}} | pending | | |
| 3 | {{phase_3}} | pending | | |

When a phase starts, create `specs/YYYY-MM-DD-<feature-name>/` and record the folder name in the table. When it closes, set the status to `complete` and add a dated line below.

## Log

Newest first. One line per phase transition: what closed, when, and the evidence.

- {{date}}: Roadmap created. Phase 1 set to in-progress.
