---
name: "Yardstick CLI"
description: "Operate the Yardstick applicant tracking system: jobs, pipelines, candidates, interview plans, scorecards, and hiring emails. Use when an agent needs to read or update hiring data, prepare interview plans, or run recruiting workflows with human-approved gates on sensitive writes."
---

# Yardstick CLI

Yardstick is the Applicant Tracking System for humans and agents. The
`yardstick` binary is a thin wrapper over the public Yardstick API: the same
read and write operations you can call from `curl` are available as named
subcommands, printed as structured JSON on stdout.

- **Website**: https://yardstick.team
- **Docs**: https://portal.yardstick.team/docs/cli (verified 2026-08-28: returns 200 publicly; app.yardstick.team/docs/cli 308-redirects here)

## Installation

```bash
npm install -g @yardstick/cli
yardstick --help
```

Note: the packaged CLI currently stores credentials in the macOS Keychain, so
interactive login is macOS-only. Headless environments on any platform can use
the env-var token fallback below.

## Authentication

Interactive login opens the Yardstick portal for approval and stores the
credential in the macOS Keychain:

```bash
yardstick login --mode test --label "Local agent"
yardstick whoami     # confirm org and effective scopes
yardstick status     # inspect the saved credential without printing the token
yardstick logout
```

Use `--mode test` for development (test-partition data, never mutates live
data) and `--mode live` for production. Named profiles via `--profile <name>`.
Add scopes later with `yardstick reauthorize --add-scopes <scope,scope>`.

For CI and other headless shells, set an explicit token:

```bash
YARDSTICK_API_TOKEN=<token> \
  YARDSTICK_API_ENDPOINT_MODE=api-origin \
  YARDSTICK_API_BASE_URL=https://api.yardstick.team \
  yardstick whoami
```

## Key Commands

```bash
yardstick help topics                          # discover command families (local metadata, safe pre-login)
yardstick jobs list --limit 5
yardstick people create --email ada@example.com --name "Ada Lovelace"
yardstick applications create --person-id per_123 --job-posting-id jp_123 --idempotency-key application-123
yardstick applications list --limit 25
yardstick interview-plans read --interview-plan-id ipd_123
yardstick interview-sessions scorecard submit --interview-session-id is_123 --body-json '{"interviewer_user_id":"...","ratings":[...]}'
yardstick events list
yardstick audit-log list
yardstick mcp serve                            # local stdio MCP server over the same credential
```

Sensitive mutations use a two-step preview/bind flow: the preview returns the
binding requirements without mutating anything, and the bind succeeds only
after a human has approved the action:

```bash
yardstick applications transition-preview --help
yardstick job-postings publish-preview --help
```

## Agent-Friendly Features

- Structured JSON on stdout; API errors preserved verbatim (status code, error
  shape, request id) and the CLI exits non-zero on any non-2xx response, so
  shell pipelines fail loud
- Help system is local metadata: `yardstick help <topic>` never touches
  credentials, the API, or npm, so it is safe inside an agent planning step
- Idempotency keys on writes, with a key fingerprint echoed in every write
  response
- Approval-required writes use a preview then bind pair, so an agent can
  prepare work and a human approves it before it commits
- Scoped credentials: `yardstick whoami` returns effective scopes, and a 403
  means under-scoped, fixable with `yardstick reauthorize --add-scopes`
- Test mode targets an isolated test partition and never mutates live data
- `yardstick mcp serve` exposes the same operations as a local stdio MCP
  server for MCP-native agents
