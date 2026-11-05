---
name: "Nexscope CLI"
description: "Ecommerce tool discovery, bounded MCP API calls, and offline product comparison reports from the terminal. Use when an agent needs Amazon/TikTok Shop/Shopify research tools, schema inspection, dry-run MCP calls, or local product comparison reports."
---

# Nexscope CLI

Ecommerce tool discovery, bounded MCP API calls, and offline product comparison reports from the terminal.

- **Website**: https://www.nexscope.ai/
- **Repository**: https://github.com/nexscope-ai/nexscope-cli

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/nexscope-ai/nexscope-cli/main/install.sh | sh
```

Or with pip (Python ≥ 3.11):

```bash
pip install nexscope-cli
```

## Key Commands

### Discover tools (offline)

List and inspect available ecommerce data tools without calling the network.

```bash
nexscope tools
nexscope tools describe <tool-name>
nexscope doctor
```

### Preview MCP calls

Always preview with `--dry-run` before executing online MCP calls.

```bash
nexscope <command> --dry-run
```

### Execute MCP calls (online)

Requires auth env vars (see below). Never pass `--allow-paid` or `--allow-write` without explicit user approval.

```bash
nexscope <command> --execute
```

### Offline product comparison reports

Generate local product comparison reports from local data without an API key.

```bash
nexscope report
```

## Authentication (online only)

```bash
export NEXSCOPE_API_KEY=...
export NEXSCOPE_PROXY_BASE=https://api.nexscope.ai/
```

Offline discovery, schema inspection, and local reports work without an API key.

## Agent Rules

- Always run with `--dry-run` before any online `--execute`.
- Never pass `--allow-paid` or `--allow-write` without explicit user approval.
- Confirm `NEXSCOPE_API_KEY` and `NEXSCOPE_PROXY_BASE=https://api.nexscope.ai/` before online calls.
- Prefer offline tool discovery and local reports when an API key is unavailable.
