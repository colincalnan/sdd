---
title: "{{project_title}}: Tech Stack"
type: constitution
updated: {{date}}
---

# Tech Stack

*Part of the constitution. The single source of truth for every technology and architecture decision. The agent re-reads this before introducing any new pattern or dependency, and may not violate it as a side effect of building a feature. Changing it is a deliberate, logged decision.*

## Stack

| Layer | Choice | Notes |
|---|---|---|
| Language | {{language}} | |
| Framework | {{framework}} | |
| Package manager | {{package_manager}} | |
| Test runner | {{test_runner}} | |
| Data store | {{data_store}} | |

## Architecture rules

Rules the agent must never violate. Written as hard constraints, not preferences.

- {{rule_1}}
- {{rule_2}}
- {{rule_3}}

## Boundaries

What the agent must never touch or change without explicit instruction (secrets, migrations, external contracts, generated files):

- {{boundary_1}}

## How done is proven here

The validation bar that every phase's `validation.md` inherits by default:

- {{validation_bar}}
