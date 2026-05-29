---
name: "Longbridge Terminal"
description: "AI-native CLI for Longbridge Securities. Query real-time quotes, charts, fundamentals, portfolio, options, and sector data for HK/US/A-share/SG markets. All data commands support --format json. Use when the user asks about stocks, market data, or their Longbridge portfolio."
---

# Longbridge Terminal

[Longbridge Terminal](https://github.com/longbridge/longbridge-terminal) is an AI-native CLI for Longbridge Securities — a major securities broker serving HK, US, A-share (SH/SZ), and Singapore markets. It exposes 125+ sub-skills as direct CLI commands with structured `--format json` output, making it an ideal tool for AI agents.

## Installation

```bash
# macOS (Homebrew)
brew install longbridge/tap/longbridge-terminal

# Linux / Windows (via npm)
npx longbridge-terminal

# Or download from GitHub Releases
# https://github.com/longbridge/longbridge-terminal/releases
```

## Authentication

```bash
longbridge auth login          # Opens browser OAuth (read-only market data)
longbridge auth login --trade  # With Trade scope (portfolio, account)
longbridge auth status         # Verify authentication
```

## Key Commands

The CLI is self-documenting. Always run `--help` to get current subcommand names and flags — they evolve with the platform.

```bash
longbridge --help                    # List all available subcommands
longbridge <subcommand> --help       # Get flags for a specific subcommand
longbridge <subcommand> --format json  # Structured output for agent parsing
```

### Common workflows

```bash
# Real-time quote with valuation
longbridge quote NVDA.US --format json
longbridge quote 700.HK --format json

# Candlestick / OHLCV data
longbridge kline AAPL.US --period day --format json

# Company fundamentals
longbridge fundamental 600519.SH --format json

# Analyst ratings and price targets
longbridge analyst-rating TSLA.US --format json

# Portfolio overview (requires --trade scope)
longbridge portfolio --format json

# Options chain
longbridge option-chain NVDA.US --format json

# Capital flow (intraday)
longbridge capital-flow 700.HK --format json

# Sector rankings
longbridge sector-ranking --market hk --format json

# Earnings calendar
longbridge calendar --type earnings --format json
```

## Agent-Ready Traits

| Trait | Detail |
|-------|--------|
| Structured output | `--format json` on all data commands |
| Non-interactive auth | Token-based via `longbridge auth login` |
| Non-interactive mode | No prompts during data queries |
| Meaningful exit codes | 0 = success, non-zero = error |
| Self-describing | `--help` on every subcommand |
| Piped output | Auto-detects TTY; JSON when piped |

## Error Handling

| Situation | Action |
|-----------|--------|
| `command not found: longbridge` | Ask user to install; see installation section above |
| stderr `not logged in` / `unauthorized` | Run `longbridge auth login` (add `--trade` for account features) |
| `rate limit` in stderr | Wait and retry; surface the message to user |
| Other errors | Surface verbatim — never silently retry |

## Markets and Symbol Format

| Market | Example symbols |
|--------|----------------|
| US | `AAPL.US`, `NVDA.US`, `TSLA.US` |
| Hong Kong | `700.HK`, `9988.HK`, `3690.HK` |
| A-share Shanghai | `600519.SH`, `000001.SH` |
| A-share Shenzhen | `300750.SZ`, `002415.SZ` |
| Singapore | `D05.SGX`, `U11.SGX` |
| Crypto | `BTC.HAS`, `ETH.HAS` |

## Skill Collection

For 125+ higher-level agent skills built on top of this CLI (DCF models, options strategies, portfolio diagnosis, earnings previews, etc.), install the skill pack:

```bash
/plugin marketplace add longbridge/skills
# or
npx skills add https://github.com/longbridge/skills
```
