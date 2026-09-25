---
name: "AgentLinkOps CLI"
description: "Check whether a page links to a target, keep backlink and AI-citation observations as dated evidence in a repository ledger, and import supplier backlink exports. Use it when an agent needs to verify a placement, track links over time or record which sources an AI answer cited."
---

# AgentLinkOps CLI

`agentlinkops` checks backlinks from your terminal and keeps what it saw in a `.agentlinkops/` ledger inside your repository. Every observation carries its evidence and date, so an agent can show why it reported a link as present, absent or unknown.

## Install

```bash
npm install -g agentlinkops        # Node.js 22.13 or newer
brew install sunlover7/agentlinkops/agentlinkops
```

The package is open source under Apache-2.0: https://github.com/sunlover7/agentlinkops-cli

## When to use it

- You placed a link and want to confirm the page carries it, with the anchor and `rel` values found.
- You track expected links across many pages and need to know which ones disappeared.
- You have a backlink export from Ahrefs, Semrush, Majestic, Moz, DataForSEO, Linkody, Google Search Console, Bing Webmaster Tools or a CSV and want one ledger.
- You want a fixed-sample record of which sources an AI answer cited for a prompt.

## Local commands (no account)

```bash
# One check: no ledger, no account
agentlinkops check --source https://example.com/post --target https://yoursite.com --scope domain --json

# Keep a ledger in this repository
agentlinkops init
agentlinkops add --source https://example.com/post --target https://yoursite.com --intent expected
agentlinkops check --json          # check every expected link in the ledger
agentlinkops status --json         # entries whose observations disagree with what you expect
agentlinkops diff --json           # recorded state changes, such as present to absent
agentlinkops report --out report.html

# Import a supplier export
agentlinkops import links.csv --target yoursite.com --from ahrefs

# Referring-domain mix of the ledger
agentlinkops mix --json
```

`agentlinkops setup --plan --goal verify-links` prints a read-only setup plan without an account. Walkthrough: [check one backlink locally](https://docs.agentlinkops.com/guides/first-result).

## Reading results

Commands that take `--json` print machine-readable output. A single `check` returns the observed state (`present`, `absent` or `unknown`), the reason, HTTP status, redirects, robots directives and every matching occurrence.

Exit codes:

- `0`: every expected link was present.
- `1`: the check found an expected link absent, with complete evidence.
- `2`: usage, configuration or ledger error.

An unknown result never fails the run. A blocked or incomplete fetch stays unknown, and the CLI never reports it as a removed link.

## AI citation checks

`agentlinkops citation run PANEL.json --json` runs one fixed-sample epoch for a panel of prompts and targets and writes the evidence under `.agentlinkops/citations/`. The built-in mock engine makes no paid calls. Live engines need their own setup, described in the docs.

## Hosted service (optional)

The same CLI can sync with the hosted AgentLinkOps service for scheduled monitoring. It needs `AGENTLINKOPS_TOKEN` or `AGENTLINKOPS_API_KEY`; the local commands above never do. `agentlinkops tools` lists the hosted commands, `agentlinkops describe NAME` prints one command's schema and `agentlinkops call NAME --args '{...}'` runs it. Hosted usage follows the service's own plans. See [connect with the CLI](https://docs.agentlinkops.com/guides/connect-cli).

## Agent setup

`agentlinkops skill` prints the agent reference. `agentlinkops agent setup --dry-run` shows how it would install the skill pack for detected clients (Claude Code, Codex, Cursor, Gemini CLI, Hermes); it stores no credential.

Docs: https://docs.agentlinkops.com
