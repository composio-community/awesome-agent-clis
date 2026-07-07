---
name: "Tree Ring Memory CLI"
description: "Local-first memory lifecycle for AI agents. Use when an agent needs to initialize project memory, remember durable lessons, recall prior decisions, audit sensitive or stale memory, forget or redact records, consolidate older context, or inspect memory in a terminal TUI."
---

# Tree Ring Memory CLI

Local tree-ring memory for AI agents: recall, audit, forgetting, consolidation,
evidence, framework discovery, and a terminal TUI.

- **Repo**: https://github.com/TerminallyLazy/Tree-Ring-Memory
- **Docs**: https://terminallylazy.github.io/Tree-Ring-Memory/
- **Release**: https://github.com/TerminallyLazy/Tree-Ring-Memory/releases/tag/v0.11.0

## Installation

macOS ARM64 via Homebrew:

```bash
brew tap TerminallyLazy/tree-ring
brew install tree-ring
```

From source:

```bash
git clone https://github.com/TerminallyLazy/Tree-Ring-Memory.git
cd Tree-Ring-Memory
cargo build --release -p tree-ring-memory-cli
```

## Authentication

No cloud account or API key is required. Memory is local-first and stored under
the configured root, normally `.tree-ring`.

## Agent-Ready Traits

- `--json` emits machine-readable output where supported
- `--root DIR` isolates memory per project, workspace, or test run
- Explicit write commands; no background transcript scraping
- Local SQLite/FTS storage
- Audit, redaction, deletion, and consolidation commands
- Non-destructive integration discovery with `integrations scan`

## Common Commands

Initialize project memory:

```bash
tree-ring --root .tree-ring init --json
tree-ring --root .tree-ring welcome --init --no-animation
```

Remember a durable lesson or decision:

```bash
tree-ring --root .tree-ring --json remember \
  "Use JSON output when agents need parseable recall results." \
  --event-type lesson \
  --scope project \
  --project my-project \
  --tag agent-ready
```

Recall memory:

```bash
tree-ring --root .tree-ring --json recall "agent-ready JSON output" --project my-project --limit 5
```

Audit memory quality, privacy, and integrity:

```bash
tree-ring --root .tree-ring --json audit --audit-type all
tree-ring --root .tree-ring --json audit --audit-type sensitive
```

Forget or redact memory:

```bash
tree-ring --root .tree-ring --json forget <memory-id> --mode redact --reason "Sensitive detail no longer needed"
tree-ring --root .tree-ring --json forget <memory-id> --mode delete --reason "Wrong or obsolete memory"
```

Consolidate older memory into deterministic summaries:

```bash
tree-ring --root .tree-ring --json consolidate --dry-run
tree-ring --root .tree-ring --json consolidate
```

Inspect memory in the terminal UI:

```bash
tree-ring --root .tree-ring tui
```

Discover local agent-framework markers without changing their config:

```bash
tree-ring --root .tree-ring --json integrations scan --source-root .
```

## Notes For Agents

- Recall before repeating project setup, making release decisions, changing
  memory/integration behavior, or acting on user preferences.
- Store concise lessons, decisions, warnings, and evidence references rather
  than full transcripts.
- Do not store secrets, credentials, private identifiers, or raw sensitive
  payloads. Use redacted summaries only when memory is genuinely useful.
- Treat local project docs and current source as more authoritative than old
  memory. If memory is wrong, stale, or sensitive, redact, delete, or supersede
  it instead of relying on it.
