# ADR-0001: Scope and Boundaries

## Status

Accepted

## Context

AgentLand is being introduced as a reusable agent-engineering laboratory and platform. Before any specific capability is built, the repository needs a stable statement of what it owns and what it does not own. Without this boundary, three failure modes are likely:

- **Application bleed:** AgentLand starts to carry the business logic, domain agents, and behavior of one particular project, so it ceases to be reusable.
- **Configuration bleed:** Machine-specific and personal configuration (dotfiles, workstation settings, provider credentials) is pulled into AgentLand, so it becomes unportable and unsafe to share.
- **Premature commitment:** AgentLand is welded to a single agent harness, single model provider, or single distribution mechanism before the ideas have been proven harness-neutral.

The system this repository is meant to serve runs across at least three distinct locations: the user's dotfiles, this AgentLand repository, and individual application repositories. Each of those has different owners, different lifetimes, and different purposes. The decision below states which concerns live in which layer.

## Decision

**AgentLand owns reusable agent-engineering infrastructure and knowledge. Machine-specific configuration remains in dotfiles, and application-specific behavior remains in individual project repositories.**

Restating in layers:

- **DOTFILES** own machine and runtime configuration: personal paths, environment, workstation and tooling setup, and any machine- or account-bound secrets.
- **AGENTLAND** owns the reusable agent-engineering system: agent contracts and reusable roles, context-engineering patterns, model discovery and qualification, evals and deterministic graders, model routing and escalation policies, reusable skills and commands, observability and performance measurement, adapters for agent harnesses, and reusable development-agent workflows.
- **PROJECTS** own domain-specific agents, domain rules, and business behavior for a particular application.

A separate repository was chosen deliberately. The reasons it is not placed inside dotfiles or inside an application repository are:

1. **Ownership and lifetime differ.** Dotfiles are personal and machine-bound; they change with the machine and with the user's toolchain. An application repository changes with that application's business needs and may be private to one product. AgentLand's content — the reusable agent-engineering — outlives and outlasts both, and should evolve on its own schedule against its own evidence.
2. **Reusability requires a neutral home.** Knowledge intended to be carried across projects, machines, and eventually harnesses should not be nested under a specific dotfile layout or a specific application's tree. A dedicated repository is the least-surprising place for something that is meant to be shared.
3. **Separation of secrets and configuration.** Provider credentials and machine-specific configuration must stay in the dotfiles layer where they are bound to a machine/account. Keeping AgentLand separate makes it a clean, shareable, credential-free body of work.
4. **Separation from business logic.** An application's behavior is its own concern. Building AgentLand inside one application repository would invite that application's logic to bleed into the reusable system and would entangle their lifecycles, reviews, and distribution.
5. **Independent evolution and review.** A standalone repository lets AgentLand accumulate decision records, a roadmap, and evidence under its own history, so the reusable system can be reviewed, argued about, and versioned independently of any one consumer.

## Consequences

- The boundary becomes the authoritative test for where any new piece of content, tooling, or knowledge belongs. When in doubt: is it reusable agent-engineering (AgentLand), machine-bound (dotfiles), or application-specific (a project)?
- AgentLand must stay free of provider credentials, machine-specific configuration, dotfiles, and application business logic. Introducing any of those is a boundary violation and should be rejected or relocated.
- Because the core is intended to be harness-neutral, agent/eval/routing/observability and other concepts must be defined in terms of responsibilities, with harness-specific details (e.g., OpenCode) isolated in adapters.
- The repository gains first-class status as a source of reusable knowledge and is the place where architectural decisions are recorded as ADRs.
- Dotfiles and project repositories remain the homes for configuration and business behavior respectively, and are not expected to change ownership as part of this decision.
- Future architectural work must reference or supersede this ADR, and must not silently cross the stated boundary.

## Alternatives considered

**Place the entire system inside dotfiles.** Rejected. Dotfiles are personal and machine-bound; mixing reusable agent-engineering with machine configuration makes both harder to share, easier to leak with secrets, and impossible to evolve independently of the machine. Reusable knowledge would be trapped behind a personal configuration layer.

**Place AgentLand inside an application repository.** Rejected. This ties the reusable system's lifecycle, review, and distribution to one application, invites business-logic bleed in both directions, and gives the reusable knowledge no neutral home from which to be shared across projects or harnesses.

**Build no boundary now and defer the decision.** Not considered viable, because subsequent decisions (harness, providers, distribution) are only meaningful once the ownership boundary is fixed; deferring it would let it be decided implicitly and inconsistently.
