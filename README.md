# AgentLand

A reusable agent-engineering laboratory and platform for learning, designing, evaluating, routing, and improving AI agents.

## What AgentLand is

AgentLand is a repository of reusable agent-engineering infrastructure and knowledge. It collects the patterns, contracts, evaluators, and operating principles that make it possible to build, measure, and route AI agents well — without coupling that knowledge to any single application, machine, or provider.

## Why it exists

Building agents well requires a body of shared, tested thinking: how to define agent responsibilities, how to engineer context, how to qualify a model for a job, how to grade agent output deterministically, and how to move work up and down a model ladder. Today that thinking usually lives inside the head of one engineer, inside one project, or hidden inside a single machine configuration. AgentLand gives that thinking a home where it can be read, reviewed, versioned, and reused.

## The problem it solves

Agent engineering fragments across dotfiles, application repositories, and personal machines, so the reusable knowledge does not accumulate or travel. AgentLand separates **reusable agent-engineering** from **application-specific behavior** and from **machine-specific configuration**, so the reusable part can be learned from, improved against evidence, and carried into new projects and new agent runtimes.

## High-level architecture

AgentLand sits in the middle of a three-layer boundary:

```text
DOTFILES            (machine/runtime configuration)
    │
    ▼
AGENTLAND           (reusable agent-engineering system)   ← this repository
    │
    ▼
PROJECTS            (domain-specific agents, rules, business behavior)
```

- **Dotfiles** manage a specific machine and runtime.
- **AgentLand** holds the reusable agent-engineering system — concepts, contracts, evals, routing, and skills — that is independent of any one application.
- **Projects** hold the domain-specific agents, rules, and business behavior of a particular application.

## OpenCode as the initial reference harness

AgentLand concepts are deliberately **harness-neutral**: they are defined so they do not assume any particular agent runtime. OpenCode is the first fully supported reference implementation and adapter, giving us a concrete environment to test our ideas against. The knowledge itself is meant to transfer to other environments such as Cursor and future agent runtimes.

## Project maturity

This project is **experimental and learning-oriented**. It is a foundation and a laboratory, not a finished product. The roadmap in `docs/roadmap.md` is intentionally high level at the later phases and is expected to evolve as evidence accumulates.

## Where to start reading

Read these in order:

1. `docs/architecture.md` — the three-layer boundary, harness-neutral concepts, and how OpenCode fits in.
2. `docs/roadmap.md` — the phases, what each aims to learn, and how we expect to know a phase is done.
3. `docs/decisions/0001-scope-and-boundaries.md` — the first architectural decision record and the reasoning behind the repository boundary.
4. `AGENTS.md` — the rules that govern agents working on AgentLand itself.
