---
name: "HeyGen CLI"
description: "Create AI videos, manage avatars, translate videos, and download results via the HeyGen API. Use when an agent needs to generate videos from text prompts, create avatar-based videos, translate existing videos, or automate video production workflows."
---

# HeyGen CLI

Official CLI for the HeyGen video generation API. 30+ commands auto-generated from the OpenAPI spec. All output is JSON by default.

- **Repo**: https://github.com/heygen-com/heygen-cli
- **Docs**: https://developers.heygen.com/cli
- **Install**: `curl -fsSL https://static.heygen.ai/cli/install.sh | bash`
- **Auth**: Requires `HEYGEN_API_KEY` environment variable. Get a key at https://app.heygen.com/settings/api

## Key Commands

```bash
# Create video from prompt (blocks until done)
heygen video-agent create --prompt "Make a 30-second product demo" --wait

# Create avatar video with full control
heygen video create -d '{"type":"avatar","avatar_id":"josh_lite","script":"Hello world","voice_id":"en_male"}'

# Check video status
heygen video get <video-id>

# Download completed video
heygen video download <video-id>

# List resources
heygen video list --limit 5
heygen avatar list --limit 10
heygen voice list

# Translate a video
heygen video-translate create -d '{"video":{"type":"url","url":"https://..."},"output_languages":["es"]}'

# Discover API fields (no auth required)
heygen video create --request-schema
heygen video-agent create --request-schema
heygen video get --response-schema
```

## Async Workflow

Video creation is asynchronous. Two patterns:

**Block until done (recommended):**
```bash
heygen video-agent create --prompt "Demo video" --wait
# stdout: final resource JSON with video_url when complete
# exit 4 on timeout — stdout has partial resource, stderr has the get command to poll manually
```

**Manual polling:**
```bash
heygen video create -d '{"...}'       # stdout: JSON with video_id
heygen video get <video-id>           # stdout: JSON with status field
heygen video download <video-id>      # downloads file, stdout: JSON with path
```

## Output Contract

- **stdout**: Always JSON. Even `video download` writes binary to disk and emits `{"path":"..."}` on stdout.
- **stderr**: JSON error envelope on failure: `{"error":{"code":"...","message":"...","hint":"..."}}`
- **Exit codes**: `0` ok, `1` API/network, `2` usage, `3` auth, `4` timeout under `--wait`

## Agent-Friendly Features

- JSON output on stdout by default (no `--json` flag needed)
- Non-interactive by default — set `HEYGEN_API_KEY` and nothing reads a TTY
- `--request-schema` and `--response-schema` on every command (no auth required)
- Structured error envelope on stderr with stable `code` values
- Meaningful exit codes (0/1/2/3/4) for programmatic branching
- Automatic retries on 429 and 5xx
- `--wait` blocks with exponential backoff for async jobs
- `heygen update` for self-updating
