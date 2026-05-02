---
name: "agenttrace"
description: "Inspect local AI coding agent sessions from the terminal. Use when an agent needs to summarize Claude Code, Codex CLI, Gemini CLI, Aider, Cursor exports, and related logs for token usage, cost, latency, tool failures, anomalies, diffs, or CI health gates."
---

# agenttrace

TUI and report CLI for local AI coding agent session observability. It reads local session logs and produces interactive views plus JSON, Markdown, and HTML reports for CI.

- **Repo**: https://github.com/luoyuctl/agenttrace
- **Docs**: https://luoyuctl.github.io/agenttrace/
- **Release**: https://github.com/luoyuctl/agenttrace/releases/latest

## Installation

```bash
brew tap luoyuctl/tap
brew install agenttrace
```

```bash
curl -sL https://raw.githubusercontent.com/luoyuctl/agenttrace/master/install.sh | sh
```

## Key Commands

```bash
agenttrace --demo
agenttrace --overview -f markdown -o agenttrace-overview.md
agenttrace --overview -f html -o agenttrace-overview.html
agenttrace --overview -f json -o agenttrace-overview.json
```

## CI Gates

```bash
agenttrace --overview \
  --fail-under-health 80 \
  --fail-on-critical \
  --max-tool-fail-rate 15
```

## Agent-Friendly Features

- Local-first log parsing; no upload required.
- TUI for human inspection and JSON, Markdown, and HTML outputs for agents and CI.
- Health thresholds and critical anomaly gates for automated workflows.
- Supports Claude Code, Codex CLI, Gemini CLI, Aider, Cursor exports, Hermes, OpenCode, Kimi, and Copilot-style traces.
