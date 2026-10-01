# Roadmap

This roadmap sequences the work AgentLand is expected to do. It is intentionally **high level in the later phases** and is expected to **evolve based on evidence** gathered from earlier phases. Each phase has an objective, what we expect to learn, and a short definition of done.

The roadmap is not a commitment to a specific order of completion for later work. Phases 1 and beyond are shaped by what the earlier phases reveal. If evidence contradicts a later phase, we change the phase — the evidence wins.

---

## Phase 0 — Foundations

**Objective.** Establish the repository, the architectural boundary, the principles, and the documentation that everything else builds on.

**What we expect to learn.** Whether the DOTFILES / AGENTLAND / PROJECTS boundary is coherent and holds up as the source of truth. Whether the engineering principles are a useful shared vocabulary.

**Definition of done.** `README.md`, `AGENTS.md`, `docs/architecture.md`, this roadmap, and ADR-0001 exist, are consistent with one another, and state the boundary without contradictions.

---

## Phase 1 — OpenCode reference architecture

**Objective.** Make OpenCode the first fully supported reference implementation and adapter, so the harness-neutral concepts have a concrete environment to be tested against.

**What we expect to learn.** Which concepts translate cleanly into a real harness and which resist it. Where the boundary between the harness-neutral core and the OpenCode adapter actually is.

**Definition of done.** The core concepts in `docs/architecture.md` are expressed in at least one concrete, working form in OpenCode, and the split between the repo's own `.opencode/` development config and future distributable adapter assets is demonstrated, not just described.

---

## Phase 2 — Local model registry and discovery

**Objective.** Bring local models under observation: discover what models are available locally and describe them in a shared, versioned form.

**What we expect to learn.** What a minimal, useful model description looks like (capability, size, cost, reliability) and what discovery signals matter.

**Definition of done.** A reproducible way to list and describe available local models in a form that downstream phases (qualification, routing) can consume.

---

## Phase 3 — Objective evaluation framework

**Objective.** Introduce evals and deterministic graders so agent and model output can be judged consistently instead of by feel.

**What we expect to learn.** What a deterministic grader needs to be meaningful, and where probabilistic judgment is still required despite the preference for determinism.

**Definition of done.** A small set of graders that run deterministically and produce comparable numbers for a model/agent on a fixed task, with results recorded.

---

## Phase 4 — Model qualification and routing

**Objective.** Use evals and discovery to qualify models for specific jobs and to route tasks to them, including escalation.

**What we expect to learn.** How the escalation ladder behaves in practice, and which routing signals are predictive of success.

**Definition of done.** A routing policy that chooses among qualified models for a task and can escalate to a higher-capability model when a lower one is not sufficient.

---

## Phase 5 — Context engineering

**Objective.** Treat context as a budget: define patterns for what goes into an agent's context and how it is managed.

**What we expect to learn.** Which context patterns actually improve outcome quality per unit of budget, and which patterns harm it.

**Definition of done.** A documented set of context-engineering patterns that are applied and measured against the grading from Phase 3.

---

## Phase 6 — Agent orchestration

**Objective.** Coordinate multiple agents/roles with explicit responsibilities and permission boundaries.

**What we expect to learn.** When a single agent versus multiple specialized agents is justified, and how to keep human authority explicit in multi-agent flows.

**Definition of done.** A reproducible, justified multi-agent workflow where each role's responsibility is explicit and measurable.

---

## Phase 7 — Code review and PR workflows

**Objective.** Bring AgentLand's approach to bear on the concrete workflow of code review and pull requests.

**What we expect to learn.** How agent-engineering knowledge changes the quality and cost of a review/PR loop.

**Definition of done.** A demonstrated agent-assisted review/PR workflow whose quality is evaluated against the objective framework.

---

## Phase 8 — Frontier-model escalation

**Objective.** Formalize escalation to frontier models within the ladder, with explicit cost and permission boundaries.

**What we expect to learn.** The cost/benefit threshold at which frontier escalation is worth it over local models.

**Definition of done.** A documented, evidence-justified escalation threshold and an auditable record of when and why frontier models are used.

---

## Phase 9 — Multi-project distribution

**Objective.** Make AgentLand's reusable capabilities distributable to other repositories without leaking development-only configuration.

**What we expect to learn.** Which distribution mechanism (packaging, tooling, config layout, or other) keeps development config separate from the product while remaining portable.

**Definition of done.** Adapter assets from at least one other repository are consumed from AgentLand, cleanly separated from this repo's development `.opencode/` config.

---

## Phase 10 — Observability and optimization

**Objective.** Close the loop: continuous measurement of agent and model performance to inform permanent roles and routing.

**What we expect to learn.** Which long-term signals most reliably predict agent/model performance, supporting "measure before assigning permanent roles."

**Definition of done.** Standing observability that produces the evidence used by Phases 4 and 8, and a reviewable process that promotes or demotes models/agents based on it.
