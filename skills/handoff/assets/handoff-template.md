---
id: <YYYY-MM-DD-topic-slug>
date: <YYYY-MM-DD>
title: <one line a human recognises weeks later, not a restatement of the id>
summary: <=140 chars — where the work stands and what comes next
status: <in-progress | blocked | awaiting-review | ready-to-resume | done | superseded>
repos: <absolute paths of the repos this touches, comma separated — or "none">
tags: <3-6 kebab keywords that a fuzzy search might plausibly hit>
---

# <title>

**Handoff ID:** `<id>` · resume with `/handoff-resume <id>`

## Next session focus

<What the next agent should do first. If the user supplied arguments to /handoff,
this is their words, sharpened. One paragraph or a short ordered list — this is
the section that decides whether the handoff is useful.>

## Where things stand

<Current state in a few sentences: what is done, what is half-done, what is
untouched. Name the branch and whether it is pushed. Be specific about "half-done"
— a next agent that assumes something is finished will build on sand.>

## Artifacts

<Point at things rather than restating them. Anything already written down —
a plan, a spec, an MR description, a commit — belongs here as a reference, not
as a paraphrase in this document.>

| What | Where |
| --- | --- |
| <e.g. branch> | `<name>` (pushed / local only) |
| <e.g. MR> | <url> |
| <e.g. plan / notes> | `<path>` |

## Key context

<Only what lives nowhere else: reasoning that happened in the conversation,
constraints the user stated, decisions and the why behind them. If it is already
in a file or an MR, it goes in Artifacts instead. Omit this section entirely if
everything is already captured elsewhere.>

## Already tried — don't redo

<Dead ends, approaches that failed and why, commands that don't work in this
environment. This is often the highest-value section: it is the part of the
session that produced no artifact, so it is the part that gets repeated.>

## Open questions

<Decisions the next session needs from the user, and anything you were unsure
about. If there are none, say so — "none open" is useful information.>

## Suggested skills

<Skills the next step actually needs, with a reason each. Name them as the user would
type them, *including the plugin prefix where there is one*: `/code-review:code-review`
and `/code-review` are different skills that behave differently.

Carry over what this session discovered and used to good effect — the tool and domain
skills that made the work go: the database, deploy, lint or ticketing skills you actually
reached for, written as the user types them, `/<plugin>:<skill>` where they carry a prefix.
These need no prior sanction: without them the next agent rediscovers them from scratch,
or hand-rolls a worse version of what they do.

Withhold one class: the review, audit and loop entry points — `/code-review`,
`/simplify`, `/security-review`, `/grill-me` and the like. Several of
them fan out to subagents, so starting one is a decision to spend real money, and a
suggestion here reads to the next agent as an instruction — it will run it. List one
only if the user asked for it by name in this session.

When a skill sits somewhere between those two, **ask the user before writing the
document** rather than guessing either way — they are right here, and one question is
cheaper than both a wasted rediscovery and an unwanted fan-out. And "none" is a
perfectly good answer; do not pad the section to look thorough.>

- `/<skill>` — <why, in half a line>
