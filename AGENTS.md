# AGENTS.md

This file governs agents that **work on AgentLand itself**. It defines how you should behave when contributing to this repository. It does **not** define the agent fleet that AgentLand is meant to produce or support — that is a later concern.

## Before you act

- **Read the architecture, roadmap, and the relevant ADRs before making an architectural change.** Start with `docs/architecture.md` and `docs/roadmap.md`, then read the decision record that most closely governs the change (`docs/decisions/`).
- **Distinguish recommendations from established project decisions.** If you propose something, say it is a recommendation. If a decision record already settles it, treat it as established and reference it.
- **Explain architectural tradeoffs rather than silently choosing one.** When a choice affects the boundary, the harness-neutrality, the roadmap, or a decision record, name the options and the reason for the one you pick.

## Preserve the three-layer boundary

- Keep AgentLand in its own layer: **DOTFILES -> AGENTLAND -> PROJECTS**.
- Do not push reusable agent-engineering logic down into dotfiles or a specific project.
- Do not pull application-specific behavior up into AgentLand.
- `docs/architecture.md` and ADR-0001 are the authoritative statements of this boundary.

## What must not enter this repository

- **No application-specific business logic.** AgentLand holds reusable agent-engineering, not the behavior of one application.
- **No provider credentials or secrets.** Never add, reference in example code in a way that would be committed, or otherwise introduce keys, tokens, or machine-specific secret configuration.
- **No machine-specific or personal configuration.** Dotfiles own that layer.
- **No new agent roles simply because specialization is possible.** Justify any specialization by a concrete responsibility before proposing it.

## How to work

- **Prefer small, reviewable changes.** A focused change that is easy to review is better than a large one that is hard to assess.
- **Use deterministic verification when available.** When a test, a script, or a reproducible check can confirm a change exists, use it instead of relying on vibes. Where no deterministic check exists, say so explicitly.
- **Make significant architectural decisions land as an ADR.** When a change is architectural in a way that should be remembered and argued about, write a new decision record in `docs/decisions/` following the structure of `0001-scope-and-boundaries.md`.
- **Recommend before you implement when a change affects the boundary, the harness-neutral core, or the roadmap.** Surface recommendations and tradeoffs rather than adding tooling.

## Git and remote behavior

- **Do not commit, push, merge, or create remote artifacts unless explicitly requested.** Do not open pull requests, create branches, or create issues on your own initiative. Wait to be asked.

## Scope of this phase

- Do **not** define a permanent production agent fleet.
- Do **not** add executable tooling, dependencies, scripts, CI, model configuration, or agent/commands except where the current approved roadmap slice explicitly calls for them.
- **Implement only the currently approved roadmap slice.** Do not advance into subsequent phases without explicit human approval.
