---
name: handoff
description: Compact the current conversation into an identified handoff document under ~/.claude/handoffs/ so a later session can pick the work up. Invoke it only when the user asks to hand off; never reach for it on your own.
argument-hint: "[what the next session will focus on]"
---

# Handoff

Write a handoff document for the work in this conversation, so a fresh agent with
none of this context can continue it. Every document gets a stable identifier, and
lives in one known directory — that identifier is how a future session finds its way
back here without the user having to remember a file path.

## Where handoffs live

`~/.claude/handoffs/<id>.md`, overridable with `$CLAUDE_HANDOFF_DIR`.

Outside any workspace on purpose: a handoff is about the user's work, not part of it,
so it shouldn't turn up in `git status` or need a `.gitignore` entry. And unlike the
OS temp directory, this survives reboots — a handoff the machine deleted overnight is
worse than no handoff, because the user was counting on it.

## The identifier

`YYYY-MM-DD-<topic-slug>` — e.g. `2026-08-11-cosmos-blockers`.

Two properties matter. It sorts chronologically, and the user can guess it. Weeks later
they will type `/handoff-resume cosmos-blockers` from memory of what they were doing,
not from a code they wrote down. So the slug must name **the work**: two to four kebab
words drawn from the ticket key, repo, feature, or bug at hand. `auth-token-refresh`,
`checkout-flow-rewrite`, `handoff-skill`. Never generic filler — `session`, `work`, `task`,
`continue`, `notes` describe every handoff ever written and so identify none of them.

Get the date from `date +%F` rather than assuming it. If `<id>.md` already exists,
append `-2`, `-3`, … — but first consider whether you should be updating that document
instead (see below).

## Steps

**1. Decide: new document or update an existing one?**

If this conversation began by resuming a handoff, and the work is still the same work,
update that document in place — keep its `id`, refresh `date`, `status`, and the body.
A single ID that tracks a thread of work across many sessions is far easier to follow
than a chain of near-duplicate documents. Start a new one when the work has genuinely
moved on to a different thing.

**2. Collect what actually needs carrying.**

The test for every candidate line: *would the next agent be wrong or slow without it?*

Do not restate content that already exists as an artifact — plans, specs, ADRs, issues,
commit messages, MR descriptions, diffs. Reference them by path or URL. A next agent can
read a file; what it cannot recover is the reasoning, the constraints the user stated out
loud, and the things you tried that didn't work.

Redact secrets as you go — API keys, tokens, passwords, connection strings, personal data.
This file sits on disk indefinitely and may get shared.

**3. Write it** using `assets/handoff-template.md`. The frontmatter is not decoration —
`handoff-index.sh` reads it to build listings, and `summary`/`tags` are what fuzzy lookup
matches against, so write them for someone searching, not for someone already here.

If the user passed arguments, they describe what the next session is for. Let that steer
what you emphasise: a handoff aimed at "finish the tests" should foreground test state,
not the design discussion.

**4. Refresh the index:**

```bash
~/.claude/skills/handoff/scripts/handoff-index.sh
```

It rebuilds `INDEX.md` from the documents' frontmatter and prints them all.

**5. Report back** with the ID, the path, and the literal line to type next time:

```
Handoff written: 2026-08-11-cosmos-blockers
  ~/.claude/handoffs/2026-08-11-cosmos-blockers.md

In a new session:  /handoff-resume cosmos-blockers
```

## What makes one of these good

The failure mode isn't leaving something out — it's a document that reads like a summary
of a conversation. Summaries are pleasant and useless: they narrate what happened instead
of saying what to do. Write for an agent that will act within its first minute, and check
the result against one question: *could someone who has never seen this conversation take
the next step, correctly, from this document alone?*
