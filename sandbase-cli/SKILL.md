---
name: "sandbase-cli"
description: "Connect Cursor, Claude Code, Codex, Windsurf, Gemini CLI, OpenCode, and other coding agents to SandBase tools and AI models through MCP. Use when an agent needs to discover supported clients, install or verify SandBase MCP configuration, diagnose onboarding, or remove only SandBase-managed configuration."
---

# SandBase CLI

Configure supported AI coding clients to access SandBase's catalog of tools and models through MCP.

- **Repository**: https://github.com/sandbaseai/cli
- **Website**: https://www.sandbase.ai
- **npm**: https://www.npmjs.com/package/@sandbaseai/cli

## Installation

Run without a global install:

```bash
npx -y @sandbaseai/cli --help
```

Or install globally with Node.js 20+:

```bash
npm install -g @sandbaseai/cli
sandbase --help
```

## Onboarding Workflow

1. Inspect supported clients in structured form.
2. Connect one explicit client, or omit `--client` to configure detected clients.
3. Complete the browser authorization flow when prompted. Never expose the resulting credential.
4. Run `doctor` and verify the installed state before using tools.

```bash
npx -y @sandbaseai/cli catalog --json
npx -y @sandbaseai/cli connect --client codex
npx -y @sandbaseai/cli doctor --client codex --json
```

Common client values include `codex`, `claude-code`, `cursor`, `windsurf`, `gemini-cli`, and `opencode`. Use `catalog --json` as the authority for the installed CLI version.

## Key Commands

```bash
# Configure all safely detected clients
npx -y @sandbaseai/cli connect

# Diagnose one client
npx -y @sandbaseai/cli doctor --client cursor --json

# Remove only SandBase-managed state
npx -y @sandbaseai/cli unregister --client cursor
```

Run `unregister` only when the user explicitly asks to disconnect SandBase. The CLI preserves unowned configuration and rolls back incomplete changes.

## Agent-Friendly Behavior

- Use `--json` for machine-readable output.
- Expect non-TTY execution to select JSON output automatically.
- Parse stdout as data and stderr as diagnostics or progress.
- Branch on the process exit code: `0` for success and nonzero for failure.
- Do not modify client configuration manually after a CLI error; report the error and preserve rollback guarantees.

## Security

- Authorization uses a browser/device flow; pause for the user when interactive approval is required.
- Credentials are stored locally with restricted file permissions.
- Never print, commit, or pass SandBase credentials in command-line arguments.
- Treat ownership conflicts as a stop condition; do not overwrite third-party MCP entries.
