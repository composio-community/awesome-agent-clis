---
name: "X Twitter Scraper CLI"
description: "Command line X/Twitter API client for tweet search, profile tweets, follower export, media workflows, monitors, webhooks, and posting automation. Use when an agent needs scriptable X/Twitter data or actions through Xquik."
---

# X Twitter Scraper CLI

Command line client for the Xquik X Twitter Scraper REST API. Use it for tweet search, advanced search queries, profile tweets, user lookup, follower export, media workflows, monitors, webhooks, giveaway draws, and posting automation.

- **Repo**: https://github.com/Xquik-dev/x-twitter-scraper-cli
- **Docs**: https://docs.xquik.com/sdks/cli
- **API**: https://xquik.com

## Installation

Requires Go 1.22 or later:

```bash
go install github.com/Xquik-dev/x-twitter-scraper-cli/cmd/x-twitter-scraper@latest
```

If the command is not on `PATH`, add the Go bin directory:

```bash
export PATH="$PATH:$(go env GOPATH)/bin"
```

## Authentication

Use an API key from Xquik:

```bash
export X_TWITTER_SCRAPER_API_KEY="xq_..."
x-twitter-scraper --version
```

OAuth 2.1 bearer tokens are also supported for flows that require them:

```bash
export X_TWITTER_SCRAPER_BEARER_TOKEN="..."
```

Agents should read keys from environment variables or an approved secret store. Never print keys, commit them, or place them in shell history.

## Key Commands

```bash
# Search tweets with structured output
x-twitter-scraper x:tweets search \
  --q "from:example" \
  --limit 10 \
  --format json

# Get profile tweets
x-twitter-scraper x:tweets user-tweets \
  --username example \
  --limit 20 \
  --format json

# Export followers
x-twitter-scraper x:users followers \
  --username example \
  --limit 100 \
  --format jsonl

# Create a monitor
x-twitter-scraper monitors create \
  --kind search \
  --query "x api sdk" \
  --format json
```

Use `x-twitter-scraper --help` and resource-level `--help` output to discover generated commands and flags.

## Output Modes

Use `--format` for successful responses:

- `json` for structured objects
- `jsonl` for line-delimited streaming or batch processing
- `yaml` for readable structured output
- `raw` for unmodified response bodies
- `pretty` or `auto` for terminal use

Use `--format-error json` when an agent needs machine-readable error handling.

## Agent-Friendly Features

- API key auth via `X_TWITTER_SCRAPER_API_KEY`
- Resource-based commands for generated API coverage
- `json`, `jsonl`, `yaml`, `raw`, and pretty output formats
- `--transform` and `--transform-error` for GJSON field extraction
- File argument support with `@file` syntax for media workflows
- Non-interactive command execution suitable for scripts and coding agents

## Safety Notes

- Confirm with the user before posting, replying, liking, reposting, following, sending DMs, changing profiles, or creating webhooks.
- Prefer read-only commands for exploration and audits.
- Do not use `--debug` in agent logs because debug output can include request and response details.
- Keep API keys and bearer tokens out of prompts, logs, issues, and commits.
