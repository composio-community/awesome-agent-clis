---
name: "KubeStellar Console kc-agent"
description: "Bridge AI coding agents to Kubernetes clusters via MCP. Use when an agent needs to list pods, check cluster health, inspect deployments, view logs, or manage multi-cluster workloads."
---

# KubeStellar Console kc-agent

Kubernetes multi-cluster management from the terminal via MCP (Model Context Protocol).

- **Repo**: https://github.com/kubestellar/console
- **Docs**: https://kubestellar.io

## Installation

```bash
brew tap kubestellar/tap
brew install kc-agent
kc-agent --help
```

Or run directly:

```bash
go install github.com/kubestellar/console/cmd/kc-agent@latest
```

## Key Commands

```bash
# Start the MCP server (bridges kubeconfig to AI agents)
kc-agent serve --port 8585

# List clusters from all kubeconfig contexts
kc-agent clusters list --json

# Get pod status across clusters
kc-agent pods list --namespace default --json

# Check cluster health
kc-agent health --json

# Stream events from all clusters
kc-agent events stream --format json

# Inspect deployments
kc-agent deployments list --namespace kube-system --json

# Get node resource utilization
kc-agent nodes list --json
```

## Agent-Friendly Features

- MCP protocol support — direct tool integration with Claude, Cursor, Copilot
- `--json` output for all commands
- Multi-cluster by default — queries all kubeconfig contexts
- WebSocket streaming for real-time events
- No interactive prompts — fully scriptable
- Token auth via `KC_AGENT_TOKEN` environment variable
- Automatic kubeconfig discovery
