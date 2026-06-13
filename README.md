<h1 align="center">Awesome Agent CLIs</h1>

<p align="center">
<a href="https://platform.composio.dev/?utm_source=Github&utm_content=AwesomeAgentCLIs">
  <img width="1280" height="640" alt="Awesome Agent CLIs" src="./banner.jpg">
</a>
</p>

<p align="center">
  <a href="https://awesome.re">
    <img src="https://awesome.re/badge.svg" alt="Awesome" />
  </a>
  <a href="https://makeapullrequest.com">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square" alt="PRs Welcome" />
  </a>
  <a href="https://creativecommons.org/publicdomain/zero/1.0/">
    <img src="https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg?style=flat-square" alt="License: CC0-1.0" />
  </a>
</p>

<p align="center">A curated list of CLIs built for humans and AI agents.</p>

<div>
<p align="center">
  <a href="https://twitter.com/composio">
    <img src="https://img.shields.io/badge/Follow on X-000000?style=for-the-badge&logo=x&logoColor=white" alt="Follow on X" />
  </a>
  <a href="https://www.linkedin.com/company/composiohq/">
    <img src="https://img.shields.io/badge/Follow on LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="Follow on LinkedIn" />
  </a>
  <a href="https://discord.com/invite/composio">
    <img src="https://img.shields.io/badge/Join our Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Join our Discord" />
  </a>
</p>
</div>

---

## Quickstart: Give Your Agent 1000+ Tools

The Composio CLI lets agents search, authenticate, and execute tools across 1000+ apps from the terminal.

```bash
curl -fsSL https://composio.dev/install | bash
composio login
composio search "star a github repo"
composio execute GITHUB_STAR_A_REPOSITORY_FOR_THE_AUTHENTICATED_USER -d '{"owner":"composiohq","repo":"composio"}'
```

Get your API key at [platform.composio.dev](https://platform.composio.dev/?utm_source=Github&utm_content=AwesomeAgentCLIs)

---

## How Skills Work

Every CLI in this list has a **`SKILL.md`** file in its own folder. Skills teach AI coding agents (Claude Code, Cursor, Codex, etc.) how to install, authenticate, and use each CLI — so the agent can take real actions on your behalf.

Skills use **progressive disclosure** for efficiency:

1. **Metadata loading** (~100 tokens): The agent scans skill names and descriptions to find relevant CLIs
2. **Full instructions** (<5k tokens): Loaded only when the agent determines the skill applies
3. **Execution**: The agent runs CLI commands based on the skill's instructions

To use a skill, copy the CLI's folder into your agent's skills directory (e.g. `~/.claude/skills/`) or point your agent at this repo.

```
awesome-agent-clis/
├── gogcli/SKILL.md          # Google Suite CLI
├── stripe-cli/SKILL.md      # Stripe CLI
├── ntn/SKILL.md             # Notion CLI
├── ...                      # one folder per CLI
└── README.md
```

---

## What Makes a CLI Agent-Ready?

Not every CLI is built with agents in mind. The 🤖 badge marks CLIs that were **explicitly designed** for AI agent use. Common traits:

| Trait                                              | Why it matters                                |
| -------------------------------------------------- | --------------------------------------------- |
| Structured output (`--json`)                       | Agents parse JSON, not ASCII tables           |
| Non-interactive mode (`--no-interactive`, `--yes`) | Agents can't answer prompts                   |
| API key auth (`--key`, env vars)                   | No browser OAuth dance                        |
| Idempotency keys                                   | Agents can safely retry on failure            |
| Piped output detection                             | Auto-switches from pretty to machine-readable |
| Meaningful exit codes                              | Agents need to branch on success/failure      |

CLIs without the 🤖 badge are **established tools that work well with agents** thanks to structured output, scriptability, or good defaults.

---

## Contents

- [Quickstart](#quickstart-give-your-agent-1000-tools)
- [How Skills Work](#how-skills-work)
- [What Makes a CLI Agent-Ready?](#what-makes-a-cli-agent-ready)
- [CLIs](#clis)
  - [Tool Orchestration](#tool-orchestration)
  - [Search & Research](#search--research)
  - [Service Provisioning](#service-provisioning)
  - [Communication](#communication)
  - [Payments & Commerce](#payments--commerce)
  - [Food Delivery](#food-delivery)
  - [Cloud & Infrastructure](#cloud--infrastructure)
  - [Version Control](#version-control)
  - [Code Quality & Review](#code-quality--review)
  - [Database](#database)
  - [Analytics & Observability](#analytics--observability)
  - [Workspace & Productivity](#workspace--productivity)
  - [Voice & Media](#voice--media)
  - [IoT & Smart Home](#iot--smart-home)
- [Contributing](#contributing)
- [Resources](#resources)
- [License](#license)

## CLIs

### Tool Orchestration

- [Composio CLI](https://docs.composio.dev/docs/cli) 🤖 - Search, authenticate, and execute tools across 1000+ apps. Type-safe code generation, trigger listeners, and structured JSON output. [`skill`](composio-cli/SKILL.md)

### Search & Research

- [Tavily CLI](https://tavily.com) 🤖 - Web search, page extraction, site crawling, and multi-source research for agents. Results are pre-processed for LLM consumption. `--json` on all commands. [`skill`](tavily-cli/SKILL.md)

### Service Provisioning

- [Stripe Projects CLI](https://docs.stripe.com/projects) 🤖 - Provision provider-backed services from the terminal, link existing accounts, sync credentials into `.env`, and manage upgrades and billing. Supports agents with `--json`, `--no-interactive`, and `--auto-confirm`. Public preview. [`skill`](stripe-projects-cli/SKILL.md)
- [Ramp CLI](https://ramp.com) 🤖 - Agent-ready spend and finance service provisioning. [`skill`](ramp-cli/SKILL.md)

### Communication

- [Resend CLI](https://github.com/resend/resend-cli) 🤖 - Send emails, manage domains, API keys, contacts, and broadcasts. 53 commands across 13 resources. Auto-detects TTY for JSON output, natural language scheduling, idempotency keys, `resend doctor` for agent detection. _By [@zenorocha](https://github.com/zenorocha)_ [`skill`](resend-cli/SKILL.md)
- [wacli](https://github.com/steipete/wacli) 🤖 - WhatsApp CLI: sync message history, search messages, send texts and files, manage contacts and groups. `--json` for structured output. _By [@steipete](https://github.com/steipete)_ [`skill`](wacli/SKILL.md)
- [Sendblue CLI](https://sendblue.co) 🤖 - iMessage integration for agents. [`skill`](sendblue-cli/SKILL.md)
- [Kapso CLI](https://kapso.ai) 🤖 - WhatsApp messaging from the terminal. [`skill`](kapso-cli/SKILL.md)
- [Mailcoach CLI](https://mailcoach.app) 🤖 - Email campaigns and transactional email management. [`skill`](mailcoach-cli/SKILL.md)

### Payments & Commerce

- [Stripe CLI](https://github.com/stripe/stripe-cli) - Test webhooks, trigger events, tail API logs, and manage Stripe resources. `--json` flag for structured output. [`skill`](stripe-cli/SKILL.md)
- [Visa CLI](https://visa.com) 🤖 - AI agents make card-native payments programmatically. Closed beta. [`skill`](visa-cli/SKILL.md)

### Food Delivery

- [ordercli](https://github.com/steipete/ordercli) - CLI for Foodora and Deliveroo. View order history, track active orders, reorder past meals. `--json` output. _By [@steipete](https://github.com/steipete)_ [`skill`](ordercli/SKILL.md)

### Cloud & Infrastructure

- [Vercel CLI](https://github.com/vercel/vercel) - Deploy projects, manage domains and env vars, inspect logs, and sync local project settings with `vercel pull`. Supports token-based auth for CI with `--token`. [`skill`](vercel-cli/SKILL.md)
- [Railway CLI](https://github.com/railwayapp/cli) - Deploy services, manage variables, and provision databases from the terminal. [`skill`](railway-cli/SKILL.md)
- [AWS CLI](https://github.com/aws/aws-cli) - Manage AWS services. `--output json` for structured output, profile-based auth. [`skill`](aws-cli/SKILL.md)
- [Google Cloud CLI](https://cloud.google.com/sdk/gcloud) - Manage GCP resources. `--format=json`, service account auth, scriptable. [`skill`](gcloud-cli/SKILL.md)

### Version Control

- [GitHub CLI](https://github.com/cli/cli) - Issues, PRs, repos, actions, and code search. `--json` with `--jq` for precise field extraction. [`skill`](github-cli/SKILL.md)
- [GitLab CLI](https://gitlab.com/gitlab-org/cli) - MRs, issues, pipelines, and CI/CD management. `--output json` flag. [`skill`](gitlab-cli/SKILL.md)

### Code Quality & Review

- [CodeRabbit CLI](https://docs.coderabbit.ai/cli/overview) 🤖 - AI code reviews directly in your terminal before you commit. Reviews local changes, supports interactive, `--plain`, and agent-friendly `--prompt-only` modes, and pairs with coding agents for fix loops. Open beta. [`skill`](coderabbit-cli/SKILL.md)

### Database

- [Supabase CLI](https://github.com/supabase/cli) - Manage Postgres databases, auth, edge functions, and storage. Local dev with `supabase start`. [`skill`](supabase-cli/SKILL.md)
- [Neon CLI](https://neon.com/docs/reference/neon-cli) - Manage Neon projects, branches, databases, roles, and connection strings from the terminal. Supports web auth or API-key auth and structured output formats. [`skill`](neon-cli/SKILL.md)
- [PlanetScale CLI](https://github.com/planetscale/cli) - Branch, deploy, and manage MySQL databases with deploy requests. [`skill`](planetscale-cli/SKILL.md)
- [Turso CLI](https://github.com/tursodatabase/turso-cli) - Manage libSQL/SQLite databases, replicas, and groups. [`skill`](turso-cli/SKILL.md)

### Analytics & Observability

- [PostHog CLI](https://posthog.com) - Query events, manage feature flags, and pull analytics data. [`skill`](posthog-cli/SKILL.md)
- [Sentry CLI](https://github.com/getsentry/sentry-cli) - Manage releases, debug symbols, source maps, cron monitors, and logs. `--format json` output. [`skill`](sentry-cli/SKILL.md)
- [Sentry Developer CLI](https://cli.sentry.dev/) 🤖 - Interactive issue management with AI-powered root cause analysis (`sentry issue explain`), step-by-step fix plans, structured JSON output, auto-detects project from `.env`. Built for humans and agents.

### Workspace & Productivity

- [Google Workspace CLI (`gws`)](https://github.com/googleworkspace/cli) 🤖 - One CLI for all of Google Workspace. Dynamically built from Google's Discovery Service — when Google adds an API endpoint, `gws` picks it up automatically. Drive, Gmail, Calendar, Sheets, Docs, Chat, Admin, and more. 100+ agent skills included, structured JSON output, structured exit codes (0–5), helper commands (`+send`, `+agenda`, `+triage`), and Model Armor response sanitization. 22k+ stars. [`skill`](google-workspace-cli/SKILL.md)
- [gogcli](https://github.com/steipete/gogcli) 🤖 - Google Suite CLI for Gmail, Calendar, Chat, Drive, Docs, Sheets, Slides, Forms, Contacts, Tasks, Keep, and Admin. JSON output, multiple accounts, OAuth/service-account auth, command allowlist for sandboxed agent runs. 6.5k+ stars. _By [@steipete](https://github.com/steipete)_ [`skill`](gogcli/SKILL.md)
- [ntn](https://www.npmjs.com/package/ntn) - Notion CLI. Authenticate, manage Workers, interact with the Notion API (`ntn api`), and upload files. Inline request syntax for API calls. _By [@makenotion](https://github.com/makenotion)_ [`skill`](ntn/SKILL.md)
- [Remio CLI](https://remio.ai/) - Search, read, and update a local-first personal knowledge base from the terminal. Requires the Remio desktop client; the CLI and skill are agent interfaces to the client, not standalone memory backends. Pre-parsed files, webpages, recordings, emails, messages, and notes reduce repeated raw file scans and context-token cost for agents. Structured JSON output by default. [`skill`](remio-cli/SKILL.md)

### Voice & Media

- [Remotion CLI](https://www.remotion.dev/docs/cli/) - Render videos, audio, and stills programmatically with React. Scriptable via `npx remotion` commands like `studio`, `render`, `compositions`, and `bundle`. [`skill`](remotion-cli/SKILL.md)
- [ElevenLabs CLI](https://elevenlabs.io) 🤖 - Voice and audio service management from the terminal. [`skill`](elevenlabs-cli/SKILL.md)
- [gifgrep](https://github.com/steipete/gifgrep) - Search GIFs from Tenor/Giphy via CLI or TUI. Scriptable with `--json`, `--format`, `--max`, plus inline terminal previews, downloads, and still/sheet extraction. _By [@steipete](https://github.com/steipete)_ [`skill`](gifgrep/SKILL.md)

### IoT & Smart Home

- [sonoscli](https://github.com/steipete/sonoscli) - Control Sonos speakers over local network. Playback, grouping, queue, favorites, scenes, Spotify integration, and volume control. `--format json` output. _By [@steipete](https://github.com/steipete)_ [`skill`](sonoscli/SKILL.md)

## Contributing

Found a CLI that should be here? Open a PR!

1. Fork this repo
2. Add the CLI to the appropriate category in the README
3. Use the format: `- [Name](link) - One-line description. [`skill`](folder/SKILL.md)`
4. Add 🤖 if the CLI was explicitly built with agent use in mind
5. Create a folder with a `SKILL.md` file (see [How Skills Work](#how-skills-work))
6. Submit a PR

### SKILL.md Format

Each `SKILL.md` must include YAML frontmatter with `name` (≤64 chars) and `description` (≤1024 chars), followed by Markdown instructions covering installation, authentication, and key commands:

```yaml
---
name: "CLI Name"
description: "What this CLI does and when an agent should use it."
---

# CLI Name

Installation, auth, key commands, output modes...
```

### CLI Requirements

- **Publicly available** (GA or public beta)
- **Documented** with a working README or docs site
- **Useful from a terminal** (not just a wrapper around a GUI)
- **Not an AI coding agent** — this list is for CLIs that agents _use_, not agents themselves

## Resources

- [CodeRabbit CLI docs](https://docs.coderabbit.ai/cli/overview) - Review local changes, use `cr --prompt-only` for agent workflows, and integrate with Claude Code or Codex
- [CodeRabbit docs index](https://docs.coderabbit.ai/llms.txt) - Full documentation index for discovering CLI pages, integrations, and command reference
- [CodeRabbit CLI launch post](https://x.com/JuanPa/status/2037559738955694404?s=20) - Launch post from Juan Pablo
- ["Building CLIs for agents" by @ericzakariasson](https://x.com/ericzakariasson/status/2036762680401223946) - 384K-view thread on what makes CLIs work for agents: non-interactive flags, useful `--help` with examples, idempotent commands, `--dry-run`, actionable errors, and pipeable output
- [Resend CLI announcement](https://resend.com/blog/resend-cli) - "Built for humans and AI agents"
- [Stripe Projects announcement](https://stripe.com/blog/introducing-stripe-projects) - "Provision and manage services from the CLI"
- [Composio CLI docs](https://docs.composio.dev/docs/cli) - Search and execute tools across 1000+ apps
- [Awesome Claude Skills](https://github.com/composio/awesome-claude-skills) - Curated Claude skills
- [Awesome OpenClaw Plugins](https://github.com/composio/awesome-openclaw-plugins) - Community-built OpenClaw plugins

## Join the Community

- [Join our Discord](https://discord.com/invite/composio) - Chat with other developers building agent workflows
- [Follow on Twitter/X](https://x.com/composio) - Stay updated on new CLIs and agent tooling
- Questions? [support@composio.dev](mailto:support@composio.dev)

## License

[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)
