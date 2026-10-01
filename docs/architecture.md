# Architecture

This document describes the architectural boundary of AgentLand, the concepts that remain harness-neutral, how OpenCode fits in as the first reference adapter, and the conceptual areas AgentLand is expected to grow into. It also documents the foundational engineering principles that guide all of it.

This is a living document. It states what AgentLand is and is not at this stage, and it names the likely future areas **without** prematurely implementing them. When a concrete decision is made, it is recorded as an ADR in `docs/decisions/`.

## The boundary: DOTFILES -> AGENTLAND -> PROJECTS

AgentLand owns **reusable agent-engineering infrastructure and knowledge**. It deliberately does not own machine-specific configuration or application-specific behavior. The system is split into three layers:

```text
DOTFILES
machine/runtime configuration
        |
        v
AGENTLAND
reusable agent-engineering system
        |
        v
PROJECTS
domain-specific agents, rules, and business behavior
```

- **DOTFILES** — the user's machine and runtime configuration. Personal, machine-specific, and owns things like local paths, dotfiles, and environment. It is the *where a human runs things* layer.
- **AGENTLAND** — the reusable agent-engineering system. This repository. It owns contracts, patterns, evaluators, routing, skills, and observability that are not tied to one machine or one application.
- **PROJECTS** — a specific application repo. It owns domain-specific agents, domain rules, and business behavior. It is the *what a particular product does* layer.

### What AgentLand owns

Reusable agent-engineering infrastructure and knowledge, including eventually:

- agent contracts and reusable roles
- context-engineering patterns
- model discovery and qualification
- evals and deterministic graders
- model routing and escalation policies
- reusable skills and commands
- observability and performance measurement
- adapters for agent harnesses
- reusable development-agent workflows

### What AgentLand does NOT own

- application business logic
- application-specific domain agents
- personal or project secrets
- machine-specific workstation configuration
- the user's dotfiles
- provider credentials
- application source code

This distinction is the core of the architecture. If a piece of knowledge is about *how to build and operate agents well* in general, it belongs in AgentLand. If it is about *this machine* or *this application*, it belongs in dotfiles or a project respectively.

## Harness-neutral core concepts

The concepts in AgentLand are defined so that they **do not assume any one agent runtime is the only possible harness**. AgentLand is about the ideas — the contract, the context budget, the routing policy, the eval — not about the tool that executes them.

Practical consequences:

- Core abstractions should be named and documented in terms of responsibilities, not in terms of a specific harness feature.
- Anything that is truly OpenCode-specific (tool names, config schema, command syntax) lives in the OpenCode adapter, not in the core.
- The same concept should be re-expressible in another harness (for example Cursor or a future runtime) without rewriting the core idea.

This is a binding constraint on how the rest of the repository is shaped. The goal is that the knowledge and the reusable pieces **transfer across environments**, not that they are locked to the first runtime that was used.

## OpenCode as the first reference adapter

Among harnesses, **OpenCode is the first fully supported reference implementation and adapter**.

Two roles to keep separate:

1. **Developing AgentLand** — the repository's own `.opencode/` configuration is used to *develop* AgentLand: it configures the harness for the work of building this repo itself. It is local to this repository and is not the product being shared.
2. **Providing capabilities to other repositories** — future *OpenCode adapter assets* are the reusable capabilities AgentLand *provides* outward. These are the things other repositories will consume.

That is a deliberate split:

```text
.opencode/
    = configuration used to DEVELOP AgentLand

future OpenCode adapter assets
    = reusable capabilities AgentLand PROVIDES to other repositories
```

The distinction matters because it keeps the "development tooling" separate from the "distributable product." In this phase we do **not** choose a final distribution mechanism. We document the intent now so that later decisions about *how* to ship adapter assets (packaging, a tool, a config layout, a registry, etc.) are informed by this boundary rather than accidentally collapsing development config into the product.

## Likely future conceptual areas

The following areas are expected to grow out of AgentLand. They are named so the roadmap and decision records have a shared vocabulary. None of them are implemented in this phase, and none of them is being chosen in detail yet.

- **agents** — the reusable roles and contracts an agent fulfils.
- **adapters** — the bridge between the harness-neutral core and a specific runtime (OpenCode first, others later).
- **models** — the set of models available, how they are discovered, and how they are qualified.
- **evals** — the evaluators and deterministic graders used to judge agent and model output.
- **skills** — reusable skills and commands that can be attached to agents.
- **routing** — the policies that decide which model or agent handles which task, including escalation.
- **observability** — the measurement and performance signals used to decide what to trust.

Each is a candidate for its own deeper documentation and ADRs as it is worked on.

## Engineering principles

These principles are documented here because they constrain every later decision. They are the "why" behind the architecture.

1. **Local-first, not local-only.** Prefer local models and local execution when they are sufficient, but do not treat locality as a hard boundary.
2. **Evidence over vibes.** Decisions about agents and models are supported by measurement, evals, and data, not by intuition alone.
3. **Deterministic tools before probabilistic reasoning.** Use deterministic checks and graders before relying on a model's judgment.
4. **Agent specialization must be justified by responsibility.** Do not split agents into more roles than the responsibility justifies.
5. **Context is a budget.** Context length and content are a constrained resource to be managed, not an infinite one.
6. **Human authority and permission boundaries remain explicit.** What a human has authorized, and what an agent may do, must be explicit and visible.
7. **Agent harnesses and model providers should be replaceable where practical.** The design should not be welded to one harness or one provider.
8. **Applications stay clean; AgentLand infrastructure does not leak application business logic into itself.** Reusable stays reusable.
9. **Model capability is an escalation ladder, not a binary local/cloud decision.** Routing is a spectrum of capability, not a toggle.
10. **Measure agent/model performance before assigning permanent roles.** Promotion of a model or agent to a standing role requires evidence.

## Relationship to other documents

- `AGENTS.md` states how agents should behave while working on AgentLand.
- `docs/roadmap.md` sequences the work that will realize the areas named above.
- `docs/decisions/` records the concrete decisions, starting with `0001-scope-and-boundaries.md`, which formalizes the boundary documented in this file.
