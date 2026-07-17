# Working in this repo (Spec-Driven Development)

This project runs on Spec-Driven Development with the IDSD intent refinement. Read `specs/method.md` once to learn the method. Then, before acting:

1. **Read the constitution first, every session:** `specs/mission.md`, `specs/tech-stack.md`, `specs/roadmap.md`. `tech-stack.md` is the single source of truth for architecture and may not be violated as a side effect of building a feature. `mission.md` and `tech-stack.md` change only by deliberate, logged decision.
2. **Work the roadmap:** the top phase in `specs/roadmap.md` that is not `complete` is the current one.
3. **Spec before code:** never write application code for a phase without a confirmed feature spec. Use the `feature-spec` skill to start a phase. It interviews the owner for intent (what is wanted, constraints, failure scenarios, success scenarios, connections) and writes `requirements.md`, `plan.md`, and `validation.md` before anything is built.
4. **The owner owns "done":** intent and the definition of done belong to the person who wanted the outcome. When the spec is ambiguous, ask. Do not resolve it silently. Presence in the loop, not approval at the gate.
5. **Close a phase properly:** validate against `validation.md`, run the `changelog` skill, mark the phase `complete` in `specs/roadmap.md`.

Style: no em dashes. Use commas, periods, or rewrite.
