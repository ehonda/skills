---
name: handoff
description: Compact the current conversation into an identified handoff document under ~/.agents/handoffs/ so a later session can pick the work up. Invoke it only when the user asks to hand off; never reach for it on your own.
argument-hint: "[what the next session will focus on]"
---

# Handoff

Write a handoff document for the work in this conversation, so a fresh agent with
none of this context can continue it. Every document gets a stable identifier, and
lives in one known directory — that identifier is how a future session finds its way
back here without the user having to remember a file path.

## Where handoffs live

`~/.agents/handoffs/<id>.md`, overridable with `$HANDOFF_DIR`.

The directory is deliberately agent-neutral: a handoff is about the user's work, and the
next session that picks it up may not be running the agent that wrote it.

Outside any workspace on purpose: a handoff is about the user's work, not part of it,
so it shouldn't turn up in `git status` or need a `.gitignore` entry. And unlike the
OS temp directory, this survives reboots — a handoff the machine deleted overnight is
worse than no handoff, because the user was counting on it.

## The identifier

`YYYY-MM-DD-<topic-slug>` — e.g. `2026-08-11-payment-retry-storm`.

Two properties matter. It sorts chronologically, and the user can guess it. Weeks later
they will type `/handoff-resume payment-retry-storm` from memory of what they were doing,
not from a code they wrote down. So the slug must name **the work**: two to four kebab
words drawn from the ticket key, repo, feature, or bug at hand. `auth-token-refresh`,
`checkout-flow-rewrite`, `handoff-skill`. Never generic filler — `session`, `work`, `task`,
`continue`, `notes` describe every handoff ever written and so identify none of them.

Get the date from `date +%F` rather than assuming it. If `<id>.md` already exists,
append `-2`, `-3`, … — but first consider whether you should be updating that document
instead (see below).

## Keep the checkpoint bounded

A handoff records the state already established in this conversation. Use that evidence
and link existing artifacts; do not turn writing the document into a fresh investigation.
Read the template and, for an update, the existing document. Read other artifacts only
when a fact needed for the next step is missing. Do not replay the session history,
rebuild inventories, fetch live status or reconsider settled decisions just to make the
handoff more comprehensive. If a required fact is missing, check its source narrowly or
record it as unknown with the check the next session must make. Never invent a value.

Once the next action, current state, artifact references and relevant constraints are
clear, write the document. Check the saved document and index once for usable
frontmatter, correct references and a concrete next step, then report back. Revise again
only for a specific defect found in that check, not for another pass at completeness.

These bounds apply to both new documents and updates, including skills that use this
workflow as their base. Keep any additional checks and authorization decisions that a
variant explicitly requires, without expanding them into a review of the whole workflow.

## Steps

**1. Decide: new document or update an existing one?**

If this conversation began by resuming a handoff, and the work is still the same work,
update that document in place — keep its `id`, refresh `date`, `status`, and the body.
A single ID that tracks a thread of work across many sessions is far easier to follow
than a chain of near-duplicate documents. Start a new one when the work has genuinely
moved on to a different thing.

For an update, replace stale state and continuation steps, keeping only context that
still affects the next action. Do not append another chronological session summary or
archive the whole previous document unless the user asks. Reference durable artifacts
instead of carrying their contents forward again.

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
<this skill's directory>/scripts/handoff-index.sh
```

It rebuilds `INDEX.md` from the documents' frontmatter and prints them all.

**5. Report back** with the ID, the path, and the literal line to type next time:

```
Handoff written: 2026-08-11-payment-retry-storm
  ~/.agents/handoffs/2026-08-11-payment-retry-storm.md

In a new session:  /handoff-resume payment-retry-storm
```

## What makes one of these good

The failure mode isn't leaving something out — it's a document that reads like a summary
of a conversation. Summaries are pleasant and useless: they narrate what happened instead
of saying what to do. Write for an agent that will act within its first minute, and check
the result against one question: *could someone who has never seen this conversation take
the next step, correctly, from this document alone?*
