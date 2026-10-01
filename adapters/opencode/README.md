# OpenCode adapter

This directory contains **OpenCode-specific representations** of AgentLand capabilities.

AgentLand defines its reusable capabilities as harness-neutral contracts under `capabilities/`. This adapter translates those contracts into OpenCode artifacts — agent files, permission rules, and prompts. The capability contract is the source of meaning; the files here are one projection of it. Changing the harness should not require changing the concept, and changing the concept does not silently rewrite the adapter.

## What this directory is

- **Not** AgentLand's own development configuration. AgentLand's own `.opencode/` (if present) is used to *develop* this repository. `adapters/opencode/` is the set of OpenCode representations AgentLand *provides* to consuming projects.
- **Not** a distribution mechanism. The final installation/distribution mechanism for these files has not been chosen. See ADR-0002 decision 8.

## What a consuming project may do

- **Specialize** a capability: override fields, narrow permissions, tailor the prompt to the project's needs.
- **Replace** a capability entirely with the project's own OpenCode agent.
- **Not consume** a capability at all.

A consuming project remains authoritative for its domain-specific behavior. AgentLand's adapter files are defaults that the project can override or replace; where both a project and AgentLand define the same capability, **the project's explicit configuration wins**. This is ADR-0002 decisions 4, 5, and 6.

On this precedence: `OPENCODE_CONFIG_DIR` is **not** the default integration mechanism for consuming AgentLand's adapter, because its precedence can override a project's own `.opencode` behavior, which would invert the authority rule above. A project that explicitly wants that behavior may still use it, but AgentLand does not document or rely on it as the integration path (ADR-0002 decision 7).

## Mapping

| AgentLand capability | OpenCode representation |
| --- | --- |
| `capabilities/code-reviewer.md` | `adapters/opencode/agents/code-reviewer.md` |

Only capabilities that exist are listed. The `agents/` directory uses the plural form as the OpenCode convention.

## Adding a new capability

1. Define the harness-neutral contract under `capabilities/` (purpose, inputs, responsibilities, authority, output contract).
2. Translate that contract into an OpenCode agent file under `adapters/opencode/agents/`.
3. Add a row to the mapping table above.

Do not add an adapter file that has no corresponding capability contract. Do not add a capability contract that requires OpenCode-specific vocabulary.
