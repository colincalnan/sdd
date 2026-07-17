---
name: feature-spec
description: Start a new phase of work under Spec-Driven Development. Interviews the owner for intent, then writes the dated feature spec (requirements, plan, validation) before any code is written. Use when beginning the next roadmap phase.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, AskUserQuestion
---

# feature-spec: start a phase, spec first

You are starting one phase of this project. No application code is written until the owner has confirmed the spec you produce here. Read `specs/mission.md`, `specs/tech-stack.md`, and `specs/roadmap.md` before doing anything.

## 1. Branch

If this project works phase-by-phase on branches, create one now, named for the phase (for example `feature/<phase-slug>`). If it commits direct, skip this.

## 2. Intent interview

Do not write the spec from assumptions. Ask the owner for the five parts of intent, and stop if any is missing rather than filling it in yourself. This is what keeps the definition of done owned by the person who wanted the outcome.

1. **What is wanted** the outcome, in the owner's words, not implementation language.
2. **Constraints** what bounds the solution: performance, compatibility, deadlines, things that must not change.
3. **Failure scenarios** what going wrong looks like, so the plan can defend against it.
4. **Success scenarios** what done and working looks like from the owner's side.
5. **Connections** what else in the system or on the roadmap this touches.

If a real work sample or example would sharpen any answer, ask for it.

## 3. Write the feature spec

Create `specs/YYYY-MM-DD-<feature-name>/` (today's date, kebab-case name) and write three files.

### requirements.md

```
---
title: "<feature name>"
phase: <roadmap number>
status: in-progress
date: YYYY-MM-DD
---

# <feature name>

## Intent
- Wanted: <the outcome in the owner's words>
- Constraints: <what bounds the solution>
- Failure scenarios: <what going wrong looks like>
- Success scenarios: <what done and working looks like>
- Connections: <what else this touches>

## In scope
<what this phase does>

## Out of scope
<what it deliberately does not do, so the agent does not drift>

## Data models and interfaces
<schemas, types, endpoints, or contracts this phase introduces or changes>
```

### plan.md

Numbered task groups, each independently implementable and each small enough to validate on its own. No group should depend on a later group being finished first.

```
# Plan: <feature name>

1. <task group>
   - <step>
   - <step>
2. <task group>
   ...
```

### validation.md

How "done" is proven, written in terms the owner would recognise. Lean on executable checks first: a prose spec drifts as models change, but a test passes or fails through an exit code. Use prose only for what a human must judge by eye.

```
# Validation: <feature name>

## Automated
- <test or check the agent can run, with the command>

## Manual
- <walkthrough a person performs, step by step, with the expected result>

## Done when
- <the completion criteria, in the owner's terms>
```

## 4. Confirm before building

Show the owner the three files. Ask them to confirm intent and the definition of done are right. Presence in the loop, not approval at the gate: they own "done", you do not decide it for them at the end. Only after they confirm do you start implementing task groups from `plan.md`, one at a time, re-reading `specs/tech-stack.md` before any new pattern or dependency.

When the phase is validated against `validation.md`, run the `changelog` skill, then set the phase to `complete` in `specs/roadmap.md` with a dated line.
