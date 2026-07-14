---
name: "OpenAgentRelay"
description: "Call an existing local agent or automation shared over a trusted LAN with structured JSON output, capability checks, bounded conversations, and environment-variable authentication."
---

# OpenAgentRelay

Use OpenAgentRelay when a teammate exposes an existing local agent or automation as a trusted-LAN capability and provides the Relay URL, expected capability name, and access key through a private channel.

## Security Boundary

The current release is an Alpha for direct calls on a trusted LAN. It uses plain HTTP and a shared key.

- Never expose the Relay endpoint to the public internet.
- Never print, log, or return `RELAY_ACCESS_KEY`.
- Do not use it for sensitive requests or production write operations.
- Verify the expected capability name on every call.

## Install

```bash
pipx install open-agent-relay
relay version
```

Python 3.11 or newer is required. Inside an existing virtual environment:

```bash
python -m pip install open-agent-relay
```

## Authentication

Receive the access key privately and load it without placing it in shell history:

```bash
read -s RELAY_ACCESS_KEY
export RELAY_ACCESS_KEY
```

Do not include the key in command arguments, prompts, generated output, or diagnostics.

## One-Shot Call

Agents should always request JSON and verify the capability name:

```bash
relay ask \
  --target http://192.168.1.42:8787 \
  --expect-agent code-reviewer \
  --json \
  "Review the current change for correctness"
```

Successful output:

```json
{
  "capability": "code-reviewer",
  "result": "..."
}
```

Treat a nonzero exit code as failure. Do not parse human-readable output when `--json` is available.

## Conversation Continuation

Start a bounded Relay-managed conversation when follow-up questions need prior context:

```bash
relay ask \
  --target http://192.168.1.42:8787 \
  --expect-agent code-reviewer \
  --new-conversation \
  --json \
  "Review this change"
```

Save the returned `conversation_id`, then continue:

```bash
relay ask \
  --target http://192.168.1.42:8787 \
  --expect-agent code-reviewer \
  --conversation conv_... \
  --json \
  "Which issue is highest priority?"
```

Conversations are bounded in-memory transcripts, expire by default, and disappear when the Relay server restarts.

## Required Connection Details

Before calling, obtain:

- Relay URL, using the publisher's LAN address rather than `0.0.0.0`
- expected capability name
- purpose and trust scope
- access key through a private channel

Stop and ask the user if any of these values are missing or if the target is not clearly inside a trusted network.
