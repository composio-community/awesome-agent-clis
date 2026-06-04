---
name: "Tuning Engines CLI"
description: "Use the Tuning Engines CLI when an agent needs to configure governed AI inference, generate orchestration starters, sync agent/MCP/skill registries, inspect traces, handle approvals, or automate policy-related workflows."
---

# Tuning Engines CLI

Tuning Engines is a compliance, control, and cost action plane for production AI traffic. Use this CLI when working with governed OpenAI-compatible inference, orchestration starters, trace/approval workflows, registry sync, or policy automation.

## Install

Run the latest CLI without a global install:

```bash
npx tuningengines-cli@latest --help
```

Or install globally:

```bash
npm install -g tuningengines-cli
te --help
```

## Authentication

Use environment variables so commands can run non-interactively:

```bash
export TE_API_URL="https://app.tuningengines.com"
export TE_API_KEY="your-user-api-token"
export TE_INFERENCE_KEY="your-inference-key"
```

Use `TE_API_KEY` for tenant/admin API workflows such as traces, approvals, policy automation, and registry sync. Use `TE_INFERENCE_KEY` for model traffic against the OpenAI-compatible inference endpoint.

## Common Commands

Generate governed orchestration starter projects:

```bash
te orchestration init langgraph
te orchestration init temporal
```

Create or apply registry manifests for agents, MCP servers, and skills:

```bash
te registry template mcp
te registry sync --file tuning-registry.yml --dry-run
te registry sync --file tuning-registry.yml --apply
```

Inspect runtime traces and approvals:

```bash
te traces list
te traces show <run-id>
te approvals list
te approvals approve <approval-id>
te approvals deny <approval-id>
```

## Agent Notes

- Prefer dry-run commands before mutating registry or policy state.
- Never place provider API keys or customer secrets in registry manifests.
- Use request IDs and run IDs in generated examples so traces can be correlated.
- Treat approval commands as sensitive operations and only run them when the user explicitly asks.
