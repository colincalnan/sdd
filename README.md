# SDD: a Spec-Driven Development skill for coding agents

A Claude Code skill that sets any repository up for **Spec-Driven Development** and runs the build loop. Install it once, then in any project type `/sdd` and the agent will scaffold a spec-first setup (for a new or existing repo) and drive the work phase by phase.

It folds in the **IDSD** refinement: the spec is split by ownership so the person who wanted the outcome keeps owning intent and the definition of "done," and the agent never quietly decides done for them. Guiding rule: *presence in the loop, not approval at the gate.*

## Install

Clone this repository into your Claude skills directory, as a folder named `sdd`.

**For yourself, in every project** (personal skill):

```bash
git clone <this-repo-url> ~/.claude/skills/sdd
```

**For one project, shared with everyone who clones it** (project skill):

```bash
# from the root of the project you want to use SDD in
git clone <this-repo-url> .claude/skills/sdd
```

Either way, start (or restart) Claude Code so it picks up the skill. Confirm it is loaded by typing `/` and looking for `sdd` in the list.

To update later: `cd` into the cloned `sdd` folder and run `git pull`.

## Use

Open Claude Code in the repository you want to work in and type:

```
/sdd
```

That is the whole interface. What happens next depends on the repo:

**New or existing repo with no `specs/` folder (Bootstrap):** the agent reads the repo to tell new from existing, interviews you about the product, the tech stack and guardrails, and the phases you want to ship, then scaffolds the setup below. For an existing codebase it drafts the tech stack from your actual files rather than asking you to recite it. It does not touch application code during setup.

**A repo that already has `specs/roadmap.md` (Resume):** the agent reads the constitution, finds the next unfinished phase, and either starts it (interviewing you for intent, then writing the phase spec) or continues the one in progress. Saying "let's get started" or "what's next" also triggers this.

## What it scaffolds

```
<your repo>/
  specs/
    mission.md      what the product is, who it is for, why, what success looks like
    tech-stack.md   the stack and the architecture rules the agent must never violate
    roadmap.md      every phase with status: pending, in-progress, complete
    method.md       the SDD + IDSD method, self-contained
  skills/
    feature-spec/SKILL.md   start a phase: branch, intent interview, write the spec
    changelog/SKILL.md      update CHANGELOG.md before a merge
  CHANGELOG.md
```

If your repo has a `CLAUDE.md` or `AGENTS.md`, the skill appends a short block telling the agent to read `specs/` before acting. If it has neither, it creates `AGENTS.md`.

Everything scaffolded into your repo is **self-contained**: it never refers back to this skill. Anyone can clone your project and continue with any coding agent, using only the files inside it.

## The method in brief

**The constitution (permanent).** Three files in `specs/` that the agent re-reads before every session: `mission.md`, `tech-stack.md`, `roadmap.md`. Re-reading them each time is the point, it replaces the memory the agent does not carry between sessions. The constitution changes only by deliberate decision, never as a side effect of building a feature.

**Feature specs (one per phase).** Each phase gets a dated folder `specs/YYYY-MM-DD-<feature>/` with `requirements.md`, `plan.md`, and `validation.md`. No application code is written for a phase until you have confirmed its spec.

**The intent interview (the IDSD part).** Before writing a feature spec the agent asks you for five things and stops if any is missing rather than inventing it: what is wanted, constraints, failure scenarios, success scenarios, and connections to the rest of the system. The definition of done in `validation.md` is written in your terms, and leans on executable checks over prose wherever a human outside the team will not read the artifact.

**The loop.** Read the roadmap, start the next phase, interview for intent, write the spec, confirm it, implement task groups one at a time, validate, update the changelog, mark the phase complete, repeat.

## Why it works

Coding agents fail across sessions because they lose context: they drift from the goal, make contradictory architecture calls, and re-solve solved problems.

| Failure mode | How SDD handles it |
|---|---|
| Context loss between sessions | The agent re-reads the constitution before every action |
| Scope creep and drift | `requirements.md` defines what is in and out of scope |
| Architecture contradictions | `tech-stack.md` is the single source of truth for tech decisions |
| "Done" ambiguity | `validation.md` defines done, in your terms, before implementation starts |
| The agent deciding done for you | You hold intent and the definition of done, and confirm the spec first |

## Requirements

Claude Code (or another agent that reads `SKILL.md` skills). No other dependencies. The skill only reads and writes files and runs git commands you approve.

## License

Copyright (c) 2026 Colin Calnan. All rights reserved. This is proprietary intellectual property; use requires written permission. See [LICENSE](LICENSE).
