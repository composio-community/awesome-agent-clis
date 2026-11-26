---
name: "Gumlet CLI"
description: "Upload and manage videos, playlists, live streams, and image CDN caches on Gumlet. Use when an agent needs to upload a video, check transcoding status, organize assets into playlists, create a live stream, or purge cached images."
---

# Gumlet CLI

Manage Gumlet video workspaces, assets, playlists, live streams, channel viewers, image sources, and cache purges from the terminal.

- **Repo**: https://github.com/gumlet/cli
- **Command reference**: https://github.com/gumlet/cli/blob/main/COMMANDS.md
- **Official agent skills**: https://github.com/gumlet/cli/tree/main/skills

## Installation

```bash
brew install --cask gumlet/tap/gumlet     # macOS, Linux
npm install -g @gumlet/cli                # macOS, Linux, Windows
winget install Gumlet.CLI                 # Windows
curl -fsSL https://cli.gumlet.com/install.sh | sh
gumlet --version
```

## Authentication

```bash
gumlet login                  # browser OAuth; tokens stored in ~/.gumlet.yaml
export GUMLET_API_KEY=...     # CI / non-interactive; takes precedence over saved login
```

Create API keys at https://dash.gumlet.com/developer/api-keys.

## Key Commands

```bash
# Workspaces
gumlet video workspace list -o table

# Upload a local file, then check processing status
gumlet video asset upload --workspace-id WORKSPACE_ID --file ./intro.mp4 --title "Introduction"
gumlet video asset get --asset-id ASSET_ID

# List assets with filters and pagination
gumlet video asset list --workspace-id WORKSPACE_ID --status ready --page-size 20

# Playlists
gumlet video playlist list --workspace-id WORKSPACE_ID
gumlet video playlist add-asset --playlist-id PLAYLIST_ID --asset-ids ASSET_1,ASSET_2

# Live streams
gumlet video live workspace list
gumlet video live asset create --live-workspace-id LIVE_WORKSPACE_ID --title "Launch event"

# Image CDN
gumlet image source list -o table
gumlet image purge --source-id SOURCE_ID --urls "/photo.jpg,/banner.png"
```

Run `gumlet <command> --help` for every flag.

## Agent-Friendly Features

- JSON output by default (`-o json`) with raw API fields and pagination cursors; `-o table` for humans
- Progress and errors go to stderr; failed commands exit nonzero
- `--no-color` for clean logs
- API-key auth through `GUMLET_API_KEY` for headless runs
