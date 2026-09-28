---
name: tlgr
description: Read and act on a personal Telegram account from the terminal with the tlgr CLI (MTProto user account, not a bot). Use when the user wants to check unread Telegram chats, read or search message history, send, reply to, edit, forward or schedule messages, manage chats, groups, channels, contacts, folders, reactions, polls, media or stories, or receive Telegram events through a webhook. Every command answers in JSON with stable exit codes.
metadata: {"openclaw": {"requires": {"bins": ["tlgr"]}, "os": ["darwin", "linux"]}}
---

# tlgr: Telegram for agents

- **Repo**: https://github.com/tlgrcli/tlgr
- **Author**: [@tlgrcli](https://github.com/tlgrcli)

`tlgr` controls a real Telegram user account over MTProto through a local
daemon. Everything you can do in a Telegram app, you can do from a shell:
messages, chats, groups, channels, contacts, media, stories, polls and more.
The full reference is `AGENT.md` in the repository, and `tlgr schema --json`
describes every command, flag and response.

## Install and check

```bash
pipx install tlgr-cli          # the command is `tlgr`
tlgr agent whoami --json       # active account, daemon health, schema version
tlgr status --check            # non-zero exit when anything is wrong
```

The daemon starts itself on first use. If `whoami` reports no account, go to
**Logging in**.

## Rules that matter

1. **Always pass `--json`.** Errors are JSON on stdout too, and the exit code
   is the contract. `--results-only` strips the envelope and `--select a,b.c`
   projects fields.
2. **Reading can be visible.** `chat open` sends a read receipt the other side
   sees and clears the owner's unread badge. To look without a trace use
   `chat catchup`, `message list` or `chat open --no-read`. A read receipt
   cannot be taken back; `chat unread` only restores the owner's badge.
3. **Name the account when there is more than one.** Pass `-a <alias>` so a
   message never goes out from the wrong identity.
4. **Secrets never go in argv.** Use `--password-env`, `--api-hash-env`,
   `--token-env` and friends, or their `-stdin` and `-file` forms.
5. **Chats are `@username` or a numeric id.** Display names and phone numbers
   are not accepted. Put negative ids after `--`:
   `tlgr message list --json --limit 5 -- -100123456`.
6. **Preview before destructive calls** with `--dry-run`, and confirm with the
   user before deleting, leaving, blocking or sending to someone new.
7. **Respect rate limits.** Exit 7 carries `wait_seconds`; wait that long
   before retrying. Exit 13 means the answer is unknown, never treat it as a
   "no".

## Everyday loop

```bash
tlgr catchup --json                                  # every unread chat with recent messages, no read receipts
tlgr message list @alice --json --limit 20           # silent history
tlgr message search @team "invoice" --json           # search one chat
tlgr message send @alice "On my way" --json          # send
tlgr message send @alice "Yes" --reply-to 4211 --json
tlgr message edit @alice 4212 "Yes, 5pm" --json
tlgr message forward @news 88 90 --to @archive --json
tlgr chat open @alice --json                          # history plus a visible read receipt
```

Long text over 4096 UTF-16 units is refused unless you add `--split`.
`--typing-auto` shows "typing..." for a length-based duration before sending.
`--schedule TS` schedules instead of sending now.

Lists return `{items, has_more, next_cursor}`. Pass `--cursor <next_cursor>`
until `has_more` is false, or use `--all`.

## Beyond messages

Each group has `--help` and a generated page in `docs/reference/`:

| Group | Examples |
|---|---|
| `chat` | `list --unread`, `get --full`, `posters`, `members`, `archive`, `mute`, `leave` |
| `contact`, `user`, `resolve` | find, add and inspect people; turn a link or username into a peer |
| `reaction`, `poll`, `todo`, `location` | react, vote, create polls and checklists, share places |
| `media`, `sticker`, `gif`, `emoji` | upload, download, export |
| `story`, `folder`, `profile`, `privacy` | post stories, organise folders, change settings |
| `account` | `list`, `check`, `session list --unconfirmed` |

Before planning something unusual, run `tlgr agent capabilities --json`. It
separates what this build cannot do, what this account may not do (Premium,
bot-only, admin-only) and what tlgr refuses on purpose, each with a reason.

## Logging in

Login is two ordinary commands, so only reading the code needs a person:

```bash
tlgr auth send-code +15551234567 --alias main --api-id 12345 --api-hash-env TLGR_API_HASH --json
tlgr auth verify-code 12345 --alias main --json
```

Ask the user for the code Telegram sends them. Exit 4 with
`AUTH_PASSWORD_REQUIRED` means two-step verification is on: rerun
`verify-code` with `--password-env TLGR_2FA_PASSWORD` after the user sets
that variable. Never ask the user to paste a password into the chat.

## Reacting to events

To be woken by Telegram instead of polling, the daemon can push events to an
HTTP endpoint, signed with HMAC:

```bash
tlgr webhook set --url https://example.com/hooks/agent \
  --events message_new,message_edited --secret-env TLGR_WEBHOOK_SECRET --enabled
tlgr webhook test
```

`tlgr events list --json` lists all event types. Deterministic auto-replies and
forwards that need no model can run in the daemon as jobs in
`~/.tlgr/jobs.yaml`.

## Limiting what you can do

`--enable-commands message.list,message.send,chat.list` (or
`TLGR_ENABLE_COMMANDS`) allows only those operations; anything else exits 6.
Use it when the user wants you read-only or restricted to a few actions. It is
a guard rail, not a security boundary.

## Exit codes

| Code | Meaning | What to do |
|---|---|---|
| 0 | success | `meta.already: true` means it was already done |
| 2 | bad arguments | fix the call; check `tlgr schema <command> --json` |
| 3 | empty | nothing matched |
| 4 | auth needed | log in again |
| 5 | not found | wrong chat or id |
| 6 | permission denied or blocked by `--enable-commands` | do not retry |
| 7 | rate limited | wait `wait_seconds` |
| 8 | transient | retry shortly |
| 11, 12 | daemon or IPC problem | `tlgr daemon status --json`, then `tlgr daemon restart` |
| 13 | indeterminate | report as unknown |
