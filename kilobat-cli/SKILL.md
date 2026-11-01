---
name: kilobat-cli
description: Diagnose failed host or API connections, DNS, Wi-Fi and latency from a Mac using Kilobat CLI JSON output. Use when an agent needs local network evidence during development.
---

# Kilobat CLI

[Public source and command documentation](https://github.com/seifip/kilobat-cli). The CLI is GPL-3.0-only; the separate proprietary Kilobat menu-bar app is a different product.

## Installation and authentication

Requires macOS 14+. The Homebrew tap builds from source and requires Xcode 16+; a direct source build requires Swift 6.

```sh
brew install seifip/kilobat/kilobat
kilobat --version
kilobat --help
```

The CLI itself needs no account or API key. Diagnostics contact test targets and services, so local execution does not mean offline execution. Wi-Fi network names are subject to macOS Location-permission restrictions; other measurements remain available without names. When running, the companion app supplies names, filter status and per-network budgets.

## Diagnose a failed connection

For a host the user wants to check:

```sh
kilobat why example.com --json
```

Replace `example.com` with the intended host. Inspect the returned layer statuses to distinguish local network, interface, proxy, DNS and server checks. Report failed and skipped checks separately; one failed probe is not proof that a service is down for everyone. Avoid a broad scan when a single-host check answers the question.

JSON output is intended for scripts and agents. Preserve the diagnostic result separately from the process exit status. Consult `kilobat --help` for commands and options in the installed version rather than assuming unsupported flags.

## Optional MCP interface

`kilobat mcp` exposes the CLI as a stdio MCP server. In a client that supports stdio servers, the executable is `kilobat` and its argument is `mcp`. This is an alternative to invoking the CLI directly; it does not supply an LLM or require a particular model provider.

Start with diagnostics. If the user wants a system-setting change, establish the intended change before running it and respect macOS authorization prompts. Diagnostic output can contain network addresses and other local context; review it before sharing it outside the user's environment.
