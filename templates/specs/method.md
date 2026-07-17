---
title: "Spec-Driven Development (with the IDSD intent refinement)"
type: reference
updated: {{date}}
---

# Spec-Driven Development

A way of working with a coding agent where structured spec files drive the agent's behaviour instead of ad-hoc prompting. The spec is written before code. The agent reads it before acting. This replaces the memory the agent does not have between sessions, and prevents drift, scope creep, and re-solving problems already solved.

This project folds in the IDSD refinement (Intent-Driven Software Development). The plain form of SDD puts everything in one document written however the author felt that morning, and the agent fills every gap. IDSD fixes that by splitting the spec by ownership: the human who wanted the outcome keeps owning intent and the definition of done, and the agent never quietly decides "done" for them. The guiding metric is **presence in the loop, not approval at the gate**: done with the owner, not arriving at the end to bless a diff.

## The two layers

### The constitution (permanent)

Three files in `specs/`, created once and maintained throughout. The agent reads them before every session.

| File | Purpose |
|---|---|
| `mission.md` | What the product is, who it is for, why it exists, what success looks like |
| `tech-stack.md` | Technology choices and architecture rules the agent must never violate |
| `roadmap.md` | Every planned phase with status: pending, in-progress, or complete |

The constitution is immutable except by deliberate choice. Implementing a feature never edits the mission or tech-stack as a side effect.

### Feature specs (one per phase)

When a phase starts, a dated folder is created:

```
specs/YYYY-MM-DD-<feature-name>/
  requirements.md   intent, expectations, data models, interfaces
  plan.md           numbered task groups, each independently implementable
  validation.md     how done is proven: tests, checks, walkthroughs
```

## The intent interview (the IDSD part)

Before any feature spec is written, the agent interviews the owner for the five parts of intent, and stops if any is missing rather than filling it in:

1. **What is wanted** the outcome, in the owner's words, not implementation language.
2. **Constraints** what bounds the solution.
3. **Failure scenarios** what going wrong looks like.
4. **Success scenarios** what done and working looks like.
5. **Connections** what other parts of the system or roadmap this touches.

Miss any of the five and the agent invents it. The definition of done in `validation.md` is written in terms the owner would recognise, not in implementation terms, so "done" never drifts away from the person who wanted the outcome.

## Facts over prose where it counts

A prose spec is a prediction about the model, not a contract with it, and it drifts as models change. Where a human outside the team will not read the artifact, prefer executable checks (tests, contracts, invariants) that pass or fail through an exit code over prose that has to be re-interpreted. Keep prose where a human does read it: the mission, the why, onboarding. `validation.md` should lean on runnable checks first.

## The workflow loop

```
1. Read roadmap.md, find the next phase that is not complete
2. Run the feature-spec flow: branch, intent interview, write requirements/plan/validation
3. Confirm the spec with the owner before implementing
4. Implement task groups from plan.md, one at a time, re-reading tech-stack.md before new patterns
5. Validate against validation.md. Done means validated, not just implemented
6. Update CHANGELOG.md, mark the phase complete in roadmap.md, merge
7. Repeat, or replan if priorities shifted
```

## Why it works

Agents fail across sessions because they lose context: they drift from goals, make contradictory architecture calls, and re-solve solved problems.

| Failure mode | Fix |
|---|---|
| Context loss between sessions | The agent re-reads the constitution before every action |
| Scope creep and drift | requirements.md defines what is in and out of scope |
| Architecture contradictions | tech-stack.md is the single source of truth for tech decisions |
| "Done" ambiguity | validation.md defines done, in the owner's terms, before implementation starts |
| The agent deciding done for you | The owner holds intent and definition of done, and confirms the spec first |

## Ground rules

- Spec before code. No application code for a phase without a confirmed feature spec.
- The constitution changes only by deliberate, logged decision.
- Phases are independently shippable. No phase depends on a future one being started.
- Small scope through the full loop beats many phases half-specified.
- When the spec is ambiguous, ask. Do not resolve intent silently.
