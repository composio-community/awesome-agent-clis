---
name: "SimpleFunctions CLI (sf)"
description: "Query real-time prediction market world state, event probabilities, and calibrated uncertainty across live Kalshi and Polymarket contracts. Use when an agent needs situational awareness about current events, market-implied probabilities, cross-venue pricing divergences, or to evaluate a causal thesis against live market data."
---

# SimpleFunctions CLI (sf)

Terminal access to a calibrated world model for AI agents — distills live prediction markets (Kalshi + Polymarket) into structured world-state snapshots, runs continuous thesis evaluation, and surfaces edge detection across venues.

- **Repo**: https://github.com/spfunctions/simplefunctions-cli
- **Docs**: https://simplefunctions.dev
- **MCP**: `claude mcp add simplefunctions --url https://simplefunctions.dev/api/mcp/mcp`

## Installation

```bash
npm install -g @spfunctions/cli
sf login                 # browser auth
sf status                # verify setup
```

## Key Commands

```bash
sf world                          # ~800-token world state snapshot (salience-ranked)
sf world --delta                  # what changed since last check (~30 tokens)
sf world --focus energy,geo       # deep coverage on specific topics
sf ideas                          # daily S&T-style trade ideas with catalyst + risk
sf scan "<keyword>"               # cross-venue scan across thousands of markets
sf book <ticker>                  # Level 2 orderbook depth
sf edges                          # top mispricings across your theses
sf create "<thesis statement>"    # register a causal thesis
sf context <thesis-id>            # causal tree + top edges + positions
sf agent <thesis-id>              # interactive TUI agent with 30+ tools
```

## Agent-Friendly Features

- No-auth public read endpoints under `/api/agent/world` and `/api/public/*` — agents get situational awareness without login
- MCP server with 54 tools at `https://simplefunctions.dev/api/mcp/mcp`
- `nextActions` chains on every API response — pre-filled call parameters for autonomous exploration
- Delta mode: `sf world --delta` emits ~30-token diffs since last check for context-efficient polling
- `--json` output on most commands for structured parsing
- `llms.txt` and `skill.md` published at site root for agent self-orientation
- API key auth via `SF_API_KEY` env var or `sf login` browser flow
- Non-interactive mode: all read operations work without any credentials
