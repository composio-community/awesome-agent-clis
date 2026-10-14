---
name: "Peppol CLI"
description: "Create, validate, send, and receive e-invoices on the Peppol network through the e-invoice.be Access Point API. Use when an agent needs to send an invoice or credit note via Peppol, read received documents, validate UBL or Peppol IDs, or look up Peppol participants."
---

# Peppol CLI

Create, validate, send, and receive e-invoices on the [Peppol](https://peppol.org) network through the [e-invoice.be](https://e-invoice.be) Access Point API. The binary is `peppol`.

- **Repo**: https://github.com/e-invoice-be/peppol-cli
- **Docs**: https://e-invoice.be/peppol-cli
- **Upstream skill**: https://github.com/e-invoice-be/peppol-cli/tree/main/skills/peppol

## Installation

```bash
# macOS / Linux (Homebrew)
brew install e-invoice-be/tap/peppol-cli

# Any platform with Go
go install github.com/e-invoice-be/peppol-cli/cmd/peppol@latest
```

Binaries for Linux, macOS, and Windows are on [GitHub Releases](https://github.com/e-invoice-be/peppol-cli/releases).

## Authentication

An e-invoice.be account is required.

```bash
# Non-interactive (preferred for agents)
export PEPPOL_API_KEY=<your-api-key>

# Interactive
peppol auth

peppol auth status
```

`PEPPOL_API_KEY` takes priority over stored credentials.

## Key Commands

```bash
peppol document create json invoice.json --construct-pdf --json
peppol document validate <document-id> --json
peppol document send <document-id> --json

peppol inbox invoices --json --page-size 50
peppol document get <document-id> --full --json
peppol document ubl <document-id> -o invoice.xml

peppol lookup search "Company Name" --country BE --json
peppol validate peppol-id 0208:0123456789 --json
peppol validate ubl invoice.xml --json

peppol backup ./peppol-archive --json
```

Use `--help` on any command for all flags. Aliases: `doc` (document), `att` (attachment), `ws` (workspace).

## Agent-Friendly Features

- Global `--json` (`-j`) flag for structured output; errors are written as JSON to stderr in JSON mode
- API key auth through the `PEPPOL_API_KEY` env var
- `--yes` skips confirmation prompts on destructive commands (`document delete`, `document attachment delete`)
- Meaningful exit codes: `0` success, `1` general error, `2` auth error, `3` validation failed, `4` not found
- `-w <workspace>` targets a tenant per command without changing global state
- `peppol backup` is idempotent and resumes an interrupted run

## Tips

- Validate before sending: check the `is_valid` field of `peppol document validate <id> --json`.
- `document get` omits line items unless you add `--full`.
- Peppol IDs use the `scheme:identifier` format, for example `0208:0123456789`.
- List commands are paginated (default page size 20). Use `--page` and `--page-size`.
