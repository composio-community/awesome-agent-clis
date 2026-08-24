---
name: dtrailer
description: Use when recording, checking, or reversing a decision made during this coding session -- architectural choices, tradeoffs, or any "why we did X instead of Y" reasoning worth keeping with the code. Activates for phrases like "record this decision," "why did we choose," "log the reasoning," or when about to make a change that reverses an earlier documented decision.
---

# dtrailer -- Decision Trailer records

## Installation

Install with `npm install -g dtrailer`, or run directly via `npx dtrailer` with no install needed.

This repo may use `dtrailer`, a CLI for recording the reasoning behind decisions as
structured files under `.dtrailer/decisions/`, referenced from commits via a
`Decision: <PREFIX>-N` trailer.

## Before proposing a new decision

Check whether it's already been decided: `dtrailer decision search "<keyword>"`.
Don't create a duplicate record for something already on file.

## Recording a decision

```
dtrailer decision new --title "..." --reasoning "..."
```

This prints the new ID and the exact commit trailer (e.g. `Decision: DEC-7`) -- include
that trailer in the commit that acts on this decision.

## Reversing a decision -- never edit or delete an accepted record

To undo or walk back a decision that's already `accepted`, do not edit it and do not
invent a "cancelled" status. Create a new decision that supersedes it, with reasoning
for the reversal:

```
dtrailer decision supersede <OLD-ID> --title "..." --reasoning "why this reverses <OLD-ID>"
```

## Checking status

```
dtrailer decision list
dtrailer decision show <ID>
dtrailer validate
```

## If `.dtrailer/` doesn't exist yet in this repo

Ask the user before running `dtrailer init --prefix XXX` -- the prefix is a one-time,
repo-owner choice, not something to pick unprompted.

## What this skill does not do

Never run any `git` command on the user's behalf as part of using `dtrailer` -- this
tool only reads/writes files under `.dtrailer/`. Committing, and where the trailer goes
in the message, is the user's call.
