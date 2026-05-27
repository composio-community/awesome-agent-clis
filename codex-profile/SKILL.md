---
name: codex-profile
description: Use when installing, authenticating, inspecting, or switching Codex CLI and Codex Desktop profiles with isolated CODEX_HOME directories through the codex-profile CLI.
---

# Codex Profile

Use `codex-profile` when the user needs separate Codex CLI or Codex Desktop
accounts, projects, sessions, logs, plugins, connectors, and settings on the
same machine.

## Install

Install with npm:

```bash
npm install -g codex-profile
```

Or install with Homebrew:

```bash
brew install Ducksss/tap/codex-profile
```

Verify the install:

```bash
codex-profile doctor
```

## Profile Model

`codex-profile` wraps Codex's native `CODEX_HOME` support.

- `default` maps to `~/.codex`.
- Any other profile maps to `~/.codex-<profile>`.
- Each profile keeps separate auth, settings, sessions, connectors, plugins,
  caches, logs, and local Codex state.

Do not copy, print, parse, or edit `auth.json`. Use `codex-profile login` to
authenticate each profile.

## Common Commands

Create and authenticate profiles:

```bash
codex-profile init personal
codex-profile init work
codex-profile login personal
codex-profile login work
```

Run Codex CLI with a profile:

```bash
codex-profile cli personal
codex-profile cli work exec "review this repository"
```

Run Codex Desktop on macOS:

```bash
codex-profile app personal ~/Dev/project
```

Inspect state before acting:

```bash
codex-profile status
codex-profile status --json personal
codex-profile doctor --json
codex-profile path personal
```

## Agent Guidance

- Prefer `status --json` and `doctor --json` before launching a profile.
- Use `path <profile>` to inspect where a profile lives without launching Codex.
- Ask before `remove <profile>` unless the user has explicitly requested
  deletion; use `--yes` only after confirmation.
- Use `clone-config <source> <target>` only for non-secret configuration copied
  by the tool.
- Keep profile names short and explicit, such as `personal`, `work`, `client`,
  or `edu`.
