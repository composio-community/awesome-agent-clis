---
name: "Bring Your AI CLI"
description: "Use Bring Your AI when an agent needs to preview or execute a local Claude Code to Codex harness migration, audit AGENTS.md/CLAUDE.md behavior, inspect MCP config portability, or get an agent-readable quote/checkout handoff for the CLI license."
---

# Bring Your AI CLI

Bring Your AI is a local-first CLI for moving AI agent harness context between Claude Code and Codex. Use it when a user wants to migrate or audit rules, skills, hooks, MCP config, validation notes, portable memory, or instruction files before Codex edits code.

## Install

```bash
curl -fsSL https://bringyour.ai/install.sh | sh
bringyour --help
```

The remote MCP and commerce APIs are no-data discovery surfaces. Do not upload harness files, memories, user rules, API keys, GitHub handles, or project contents to the hosted endpoint.

## Common Commands

Preview a Claude Code to Codex move without changing source files:

```bash
bringyour preview --from claude-code --to codex
```

Apply the move with merge-first target behavior:

```bash
bringyour migrate --from claude-code --to codex --policy merge
```

Build a first Codex setup from public work history:

```bash
bringyour preview --build --github <github-user> --to codex
bringyour build --github <github-user> --to codex
```

Inspect the local MCP server:

```bash
bringyour mcp
```

## Agent-Readable Discovery

Use these endpoints before assuming current product, payment, or tool support:

```bash
curl -s https://bringyour.ai/llms.txt
curl -s https://bringyour.ai/.well-known/agent.json
curl -s https://bringyour.ai/.well-known/commerce.json
curl -s https://bringyour.ai/api/v1/catalog
```

Quote a license:

```bash
curl -s https://bringyour.ai/api/v1/quote \
  -H 'Content-Type: application/json' \
  -d '{"product_id":"bringyour_lifetime","quantity":1,"currency":"USD"}'
```

Start checkout:

```bash
curl -s https://bringyour.ai/api/v1/checkout \
  -H 'Content-Type: application/json' \
  -d '{"product_id":"bringyour_lifetime","payment_mode":"stripe_link"}'
```

## Safety Rules

- Source Claude Code and Codex roots are read-only during preview.
- Do not copy secret values into target MCP config; keep secret references as references.
- Treat Claude Code hooks and per-agent restrictions as validation notes when Codex cannot enforce equivalent behavior.
- Keep session artifacts as analytics or continuity material, not active instructions.
- Snapshot target roots before any write.
