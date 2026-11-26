---
name: apn
description: Use Agent Payment Node (APN) for local CLI and stdio MCP payment workflows, x402 offer inspection, and saved operation status on Apple Silicon macOS. Route signing and owner-controlled wallet setup to the foreground terminal.
---

# Agent Payment Node

APN is an MIT-licensed, local-first payment runtime for AI agents. It runs on Apple Silicon macOS with Node 24. The software is free; external services and network fees can cost money.

Source and installation documentation: https://github.com/nuanu-ai/agent-payment-node

## Installation and discovery

When installation is requested, the documented Homebrew command is:

```bash
brew install nuanu-ai/tap/apn
```

Before choosing commands, inspect the installed contract:

```bash
apn --version
apn --help
apn help --json
```

Help runs before wallet state, Keychain, policy, network, signing, or payment construction. The current source documents the raw `apn.command-manifest.v2` JSON manifest. Installed releases may differ: match the installed version, release provenance, and help output instead of assuming a source feature is available. Use `apn help` with the exact command path to discover its arguments and effects.

Operational commands emit one `apn.cli.v1` JSON envelope. Inspect `ok`, `error`, `operation`, `receipt`, and `next_actions`. Follow the installed manifest and returned next actions rather than guessing flags or treating unavailable values as zero.

## Authentication and owner setup

Help needs no account. Payment workflows use an explicitly named local or provider-managed wallet profile. Provider login stays in the foreground terminal. Never collect private keys, seed phrases, provider tokens, or browser credentials in chat or command arguments.

The owner supplies the wallet profile, admitted assets, rails, limits, recipient, and amount. Do not invent missing choices. Wallet creation/import, provider connection, new policies, and permission changes are separate owner actions; a request to inspect a quote does not authorize them.

## Inspect and recover

For a resource the user selected, discover `apn x402 inspect` in the installed help, then inspect that URL before considering a purchase. Inspection is not payment authorization.

For an existing operation ID, use the installed `operation status`, `operation resume`, and `receipt get` help to determine the next permitted step. A timeout, transaction hash, or lost response is not proof of settlement. Keep the saved operation and idempotency key; never repeat a possible payment effect to resolve uncertainty. Follow the family's recovery instructions.

Signing and execution require the documented foreground terminal flow and the owner's exact approval. APN's MCP foreground handoff returns the command for the owner to run; do not simulate that approval or bypass it.

## MCP

```bash
apn mcp config
apn mcp serve
```

Use the returned launch descriptor for a local stdio MCP client. APN does not provide a hosted remote MCP endpoint. MCP tools come from the installed command catalog, and human-only commands hand off to the foreground CLI.
