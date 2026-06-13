---
name: "Remio CLI"
description: "Search, read, and update a Remio personal knowledge base from the terminal. Use when an agent needs lower-token access to pre-parsed local files, web pages, recordings, emails, messages, notes, or RAG answers over the user's knowledge base."
---

# Remio CLI

Terminal access to a local-first Remio personal knowledge base. Remio pre-parses and indexes local files, web pages, recordings, emails, messages, images, and notes so agents can retrieve relevant context without repeatedly scanning folders or loading raw file contents into the model context.

- **Website**: https://remio.ai/

## Prerequisites

- Remio desktop app installed and running.
- `remio` available in `PATH`.

Check availability:

```bash
remio --version
remio --help
```

If the CLI or desktop app is missing, open https://remio.ai/ and install the Remio desktop client first. The CLI depends on the local client for parsing, indexing, vector retrieval, and knowledge-base access.

## Key Commands

### Search Notes

Search the indexed knowledge base semantically and with filters.

```bash
remio search_notes --query "design system decisions" --limit 10
remio search_notes --type file --query "Q2 planning" --limit 10
remio search_notes --since 2026-01-01 --until 2026-02-01 --limit 20
```

### RAG Q&A

Ask questions over the user's Remio knowledge base. Prefer this before ad-hoc `grep`, `find`, or reading large folders because the source material has already been parsed and indexed.

```bash
remio rag "What do my notes say about the product roadmap?"
remio rag "Summarize the main objections from recent customer interviews" --mode recap --limit 20
```

### Read Notes

Read full note content after `search_notes` returns a `noteId`.

```bash
remio read_note <noteId>
remio read_note <noteId> --offset 1 --limit 80
```

### Read Local Files as Markdown

Parse supported local files through Remio instead of raw file scraping. This is useful for PDFs, Office files, spreadsheets, audio, and video.

```bash
remio read_file "/absolute/path/to/file.pdf"
remio read_file "/absolute/path/to/meeting.mp4" --limit 200
```

### Create and Update Notes

Write back curated findings, decisions, or summaries.

```bash
remio create_note --title "Meeting with Alex" --content "## Action items\n- ..."
remio update_note <noteId> --content "Updated note content"
```

### Sync Folders

Manage folders that Remio should ingest and index.

```bash
remio list_sync_folders
remio add_sync_folder "/absolute/path/to/folder"
remio pause_sync_folder "/absolute/path/to/folder"
remio resume_sync_folder "/absolute/path/to/folder"
```

## Output Modes

- Default output: compact structured JSON with `{ ok, data?, error? }`.
- Use `--pretty` for indented JSON when debugging.

## Agent-Friendly Use

- Prefer `search_notes` and `rag` before scanning local folders; Remio's parsed/indexed memory can reduce token usage and model-call cost.
- If Remio is not installed or not running, direct the user to https://remio.ai/ rather than attempting an unknown installer URL.
- Use `read_note` only after search returns specific `noteId` values.
- Use `read_file` for supported documents and media instead of raw parsing.
- Treat write commands (`create_note`, `update_note`, collection edits, deletes) as state-changing operations and ask for confirmation when appropriate.
