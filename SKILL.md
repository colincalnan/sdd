---
name: sdd
description: Bootstrap and run Spec-Driven Development (with the IDSD intent refinement) in any code repo. Use when the user wants to set a project up for structured, spec-first work with a coding agent ("set up SDD here", "add spec-driven development to this repo", "start an SDD project", "scaffold specs"), or to resume one ("what's the next phase", "let's get started") in a repo that already has a specs/ constitution.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, AskUserQuestion
---

# SDD: Spec-Driven Development project runner

SDD is a way of working with a coding agent where structured spec files, written before any code, drive the agent's behaviour instead of ad-hoc prompting. This skill scaffolds that setup into a repo and runs the loop. It folds in the IDSD refinement: the spec is split by ownership so the human who wanted the outcome keeps owning intent and definition-of-done, and the agent never quietly decides "done" for them.

The method in one line: **a permanent constitution gives the agent guardrails, dated feature specs define one shippable phase at a time, and every phase starts with a short intent interview so nothing is built from assumptions.**

This skill has two modes. Decide which one applies first.

## Mode detection

1. Look for a `specs/roadmap.md` (the roadmap is the marker of an SDD repo), searching: the current working directory, then a folder the user named, then one level down.
2. Found one and the user's request is about continuing the work (or they just said "let's get started" / "what's next"): **Resume mode**.
3. Otherwise: **Bootstrap mode**.

If a `specs/` folder exists but is incomplete (missing mission, tech-stack, or roadmap), treat it as a half-finished bootstrap: fill the gaps rather than starting over.

## Bootstrap mode: set a repo up for SDD

### Step 1: read the ground first

Before interviewing, look at the target repo so the questions are informed, not generic:
- Is this a new (empty or near-empty) repo or an existing codebase? Check with `git ls-files | head` and a directory listing.
- If existing: identify the language, framework, package manager, test runner, and directory layout from the actual files (`package.json`, `pyproject.toml`, `go.mod`, lockfiles, config). You will draft `tech-stack.md` from what is really there, not from what the user remembers.
- Confirm the target repo path. Default: the current working directory. Never scaffold into this skill's own repo; scaffold into the user's target project.

### Step 2: interview

Ask these in one batch, conversationally. Do not scaffold until answered. This is the product-level intent, the constitution's raw material.

1. **Product**: what is this project, who is it for, and why does it exist? What does success look like (a metric or an observable outcome)?
2. **Tech and guardrails**: what is the stack, and what are the hard architecture rules the agent must never violate (data boundaries, patterns to follow, things it must never touch)? For an existing repo, present your drafted answer from Step 1 and ask what to correct or add.
3. **Phases**: what are the next few independently shippable phases? Each should be buildable, testable, and mergeable on its own. If the user only knows the first one, that is fine, capture it and leave the roadmap open.
4. **Working style**: does the agent work on a branch per phase and merge, or commit direct? Any test or validation bar that "done" always requires?

### Step 3: scaffold

Copy this skill's `templates/` into the target repo, preserving structure, so the repo ends up with:

```
<repo>/
  specs/
    mission.md          product intent: what, who, why, success metrics
    tech-stack.md       stack + architecture rules the agent must never violate
    roadmap.md          all phases with status: pending / in-progress / complete
    method.md           the SDD+IDSD method reference, self-contained
  skills/
    feature-spec/SKILL.md   how to start a phase: branch, intent interview, write the spec
    changelog/SKILL.md      how to update CHANGELOG.md before a merge
  CHANGELOG.md
```

Then:
- Fill `mission.md`, `tech-stack.md`, and `roadmap.md` from the interview. Replace every `{{placeholder}}`. In `roadmap.md`, list the phases the user gave, first one `in-progress` if they want to start now, rest `pending`.
- If the repo already has a `CLAUDE.md` or `AGENTS.md`, append the block from `templates/claude-md-block.md` so the agent is told to read `specs/` before acting. If it has neither, create `AGENTS.md` from that block.
- Do not touch application code in this step. Bootstrap sets up the method, it does not start building.

The `specs/` folder and repo-local `skills/` must be fully self-contained: the generated files never reference this skill or its repo. Anyone opening the target repo cold, with any coding agent, must be able to continue from the files inside.

### Step 4: hand off to the first phase

Do not stop after scaffolding. Tell the user the repo is set up, show the roadmap, and offer to start the first phase now by running the feature-spec flow (Resume mode, Step 2 below). Ask before writing a feature spec, the intent interview needs their answers.

## Resume mode: continue an SDD repo

1. Read `specs/roadmap.md` and find the next phase that is not `complete`.
2. Read `specs/mission.md` and `specs/tech-stack.md` (the constitution) before anything else. Re-reading these every session is the point: it replaces the memory the agent does not have between sessions.
3. If the current phase has no spec folder yet (`specs/YYYY-MM-DD-<feature>/`), run **the feature-spec flow**. If it has one, read its `requirements.md`, `plan.md`, and `validation.md`, report where the phase stands, and propose the next task group from `plan.md`.

### The feature-spec flow (starting a phase)

This is where the IDSD intent refinement lives. Follow `skills/feature-spec/SKILL.md` in the repo; in summary:

1. Create a branch for the phase if the working style uses branches.
2. **Intent interview.** Do not write the spec from assumptions. Ask the user for the five parts of intent, and stop if any is missing rather than filling it yourself:
   - what is wanted (the outcome, in their words)
   - constraints (what bounds the solution)
   - failure scenarios (what going wrong looks like)
   - success scenarios (what done and working looks like)
   - connections (what other parts of the system or roadmap this touches)
3. Write the dated feature spec: `requirements.md` (the intent above plus data models and interfaces), `plan.md` (numbered, independently implementable task groups), `validation.md` (how "done" is proven, favouring executable checks the agent can run over prose where a human outside the team will not read it).
4. Confirm the spec with the user before implementing. The rule is **presence in the loop, not approval at the gate**: they own intent and the definition of done, you do not decide it for them at the end.

### Implementing and closing a phase

1. Implement task groups from `plan.md` one at a time. Re-read `tech-stack.md` before introducing any new pattern or dependency, it is the single source of truth for architecture and the agent may not violate it as a side effect.
2. Validate against `validation.md`. Done means validated, not merely implemented.
3. Run the changelog flow (`skills/changelog/SKILL.md`) to update `CHANGELOG.md` before merging.
4. Mark the phase `complete` in `roadmap.md` with a dated note, and merge if using branches.

## Rules that apply in both modes

- **Spec before code.** The agent never writes application code for a phase without a feature spec that the user has confirmed. The spec is not documentation written after the fact.
- **The constitution is immutable except by deliberate choice.** Implementing a feature never edits `mission.md` or `tech-stack.md` as a side effect. Changing them is its own decision, logged.
- **Phases are independently shippable.** No phase depends on a future phase being started. Small scope through the full loop beats many phases half-specified.
- **Ownership stays with the human.** Intent and definition of done belong to the person who wanted the outcome. When something in the spec is ambiguous, ask, do not resolve it silently.
- **No em dashes** in any generated content. Use commas, periods, or rewrite.
- **Generated files are self-contained.** Never reference this skill or its repo from inside a target project.
