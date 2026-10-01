# ADR-0002: OpenCode Reference Adapter

## Status

Accepted

## Context

ADR-0001 established the DOTFILES -> AGENTLAND -> PROJECTS boundary and the principle that AgentLand's core is harness-neutral: concepts are named by responsibility, and anything truly harness-specific (e.g. OpenCode tool names, config schema, command syntax) is isolated in adapters. Architecture (`docs/architecture.md`) named OpenCode as the first reference adapter but left it as intent.

Phase 1 is the first experiment in turning one harness-neutral capability into a concrete OpenCode representation. Before writing the concrete artifact, the repository needs a decision record stating how the harness-neutral core and the OpenCode adapter relate to each other, who is authoritative, and what we deliberately do **not** do. Without these choices being named, three new failure modes are likely:

- **Harness bleed:** OpenCode's configuration vocabulary (agent frontmatter, permission keys, `mode`, etc.) leaks into the capability contracts, so the "reusable" concept can no longer move to another harness without rewriting the idea.
- **Precedence bleed:** the distribution mechanism used to ship adapter assets silently overrides a consuming project's explicit `.opencode` behavior, so a project cannot specialize or replace an AgentLand capability.
- **Fleet bleed:** the adapter becomes a place to park a growing fleet of agents and commands before the single-capability pattern is proven, contradicting ADR-0001's "justify specialization by responsibility" and principle 4.

The decision below fixes the relationship between the harness-neutral core, the OpenCode adapter, and the consuming project, and records the scope of this slice.

## Decision

**AgentLand defines reusable capabilities as harness-neutral contracts. OpenCode is the first reference adapter that translates those contracts into OpenCode-specific representations. Project repositories remain authoritative, and a project may specialize or replace any AgentLand-provided capability. The final installation/distribution mechanism is intentionally deferred. Phase 1 proves the adapter abstraction with exactly one capability.**

In concrete terms:

1. **AgentLand concepts remain harness-neutral.** Capability contracts in the core are named by responsibility (purpose, inputs, responsibilities, authority, output contract) and make no reference to any specific harness feature, tool, or configuration schema.
2. **OpenCode is the first reference adapter, not the architecture itself.** The adapter is a translation layer from a harness-neutral contract into OpenCode's representation (agent file, permission rules, prompt). It is not a place where the core idea lives.
3. **Reusable capabilities have a harness-neutral contract distinct from their OpenCode representation.** The contract (in `capabilities/`) is the source of meaning. The adapter file (in `adapters/opencode/agents/`) is one projection of that contract. The two are related but not identical; changing the harness should not require changing the core idea.
4. **Project-specific/domain behavior remains in project repositories.** Following ADR-0001, application-specific behavior does not enter AgentLand and is not duplicated into adapter files.
5. **A consuming project must be able to specialize or replace reusable AgentLand behavior.** The contract is a default, not a lock. A project may override a field, narrow permissions, replace the prompt, or replace the capability entirely with its own implementation.
6. **AgentLand must not silently override a project's explicit behavior.** Any distribution mechanism chosen later must preserve the project's own `.opencode` as authoritative when both define behavior for the same capability. If a mechanism's precedence inverts that ordering, it is disqualified by default.
7. **`OPENCODE_CONFIG_DIR` is not the default AgentLand distribution mechanism.** Its precedence can override project `.opencode` behavior, which conflicts with decisions 5 and 6. It may still be used where a project explicitly opts in, but it is not the distribution path by which AgentLand ships its adapter.
8. **The final installation/distribution mechanism is intentionally deferred.** This phase does not choose packaging, a tool, a config layout, a registry, or any other mechanism. Architecture and ADR-0001's boundary inform that later choice; the choice itself is out of scope here.
9. **Phase 1 proves the adapter abstraction with one capability before introducing more reusable agents.** The scope is deliberately minimal: a single generic, read-only code-reviewer capability, and its OpenCode representation. No orchestrator, no second agent, no commands, no skills, no model routing, no eval tooling.

## Consequences

- The repository gains a stable location for reusable capability contracts (`capabilities/`) that is structurally separate from any harness-specific projection (`adapters/<harness>/`). This separation is the test for harness-neutrality: any new content that cannot be stated without referencing OpenCode belongs in the adapter, not the contract.
- Every adapter file is expected to be traceable to a capability contract. An OpenCode agent file that has no harness-neutral contract is evidence of a boundary violation and should be relocated into `capabilities/` first, then expressed in the adapter afterward.
- Future distribution work must be evaluated against the authority rule from decision 6: the project's explicit configuration wins over AgentLand-provided defaults. A mechanism that cannot express that ordering is not acceptable unless the project explicitly opts in to a non-default precedence.
- The single-capability scope of Phase 1 is a deliberate guard against fleet drift. Before adding a second reusable agent, the team must demonstrate that one capability has been successfully consumed and that the pattern survives contact with a real project. If evidence shows the pattern does not hold, the next step is to fix the pattern, not to add another capability.
- `OPENCODE_CONFIG_DIR` remains available for individual projects that want to point OpenCode at a custom config, but AgentLand does not rely on it and does not document it as the integration mechanism. This decision is revisited only alongside the (still deferred) final distribution decision.
- The adapter is expected to be the place where OpenCode-specific syntax (frontmatter, permission keys, prompt style) is learned. If a future harness (e.g. another runtime) is added, the same capability contract should be able to drive a second adapter without changes to the contract.

## Alternatives considered

**Make OpenCode-specific configuration the AgentLand core.** Rejected. The reusable knowledge would be welded to OpenCode's vocabulary (frontmatter keys, permission keys, `mode` values, agent-file layout). ADR-0001's harness-neutral principle and architecture §"Harness-neutral core concepts" explicitly require that the core be re-expressible in another harness without rewriting the idea. Collapsing the adapter into the core would defeat the primary purpose of this repository.

**Use `OPENCODE_CONFIG_DIR` as the primary integration mechanism.** Rejected as a default because its precedence can override a project's explicit `.opencode` behavior, which directly contradicts the requirement that a consuming project be authoritative and able to specialize or replace AgentLand-provided capabilities. It remains a legitimate mechanism for a project that explicitly opts in, but it is not the distribution path for AgentLand. This is recorded as a decision (item 7) so the trade-off is visible rather than implicit.

**Immediately build a generic orchestrator + specialist fleet.** Rejected. It would assume the adapter abstraction works before we have proven it with one capability, contradicting principle 4 (specialization must be justified by responsibility) and ADR-0001's guidance to avoid premature commitment. It also collapses the Phase 1 experiment into a Phase 6 concern (orchestration). The single-capability scope is the cheapest possible proof that the harness-neutral contract / OpenCode adapter split is workable.

**Defer any concrete adapter until later.** Not chosen because Phase 1's stated objective in `docs/roadmap.md` is to "make OpenCode the first fully supported reference implementation and adapter, so the harness-neutral concepts have a concrete environment to be tested against." Without at least one concrete artifact, the harness-neutral claims remain untested and this repository stays at the level of intent it was in at the end of Phase 0. One adapter is the minimum step that turns the boundary from a diagram into a working demonstration.
