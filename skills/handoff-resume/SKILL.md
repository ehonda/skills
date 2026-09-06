---
name: handoff-resume
description: Pick up work from a handoff document in ~/.agents/handoffs/ — by identifier, by topic, or the most recent one. Invoke it only when the user asks to resume a handoff; never reach for it on your own.
argument-hint: "[handoff id or topic — omit for the most recent]"
---

# Handoff Resume

Find the handoff document the user means, verify it still describes reality, and get to
work on it. Companion to `/handoff` (see the `handoff` skill's own `SKILL.md` for how these
documents are written).

## Step 1 — Resolve which handoff

Handoffs live in `~/.agents/handoffs/` (or `$HANDOFF_DIR`). The lookup script belongs to
the companion `handoff` skill, not to this skill. Find that skill's installed directory,
then use its script rather than looking for `handoff-resume/scripts/handoff-index.sh`.
For example, if this skill is installed at `<skills-root>/handoff-resume/SKILL.md`, the
script is at `<skills-root>/handoff/scripts/handoff-index.sh`. Confirm that path is a file
before running it. The script parses each document's frontmatter and returns tab-separated
`ID · DATE · STATUS · TITLE · PATH · SUMMARY`:

```bash
HI=<the handoff skill's directory>/scripts/handoff-index.sh

"$HI" --find "<argument>"   # user named something — id, partial id, or topic
"$HI" --latest              # user said nothing
"$HI" --list                # show everything
```

Exit code 3 means nothing matched.

Interpret the result rather than taking the first row:

- **Exactly one match** → that's it, proceed.
- **Several matches** → don't guess. Show them as a short list (id, date, status, title)
  and ask which one. Resuming the wrong thread wastes a whole session, and the user
  recognises the right one instantly from a title.
- **Nothing matched a query** → re-run with `--list` before concluding it doesn't exist;
  the user's wording may not overlap the slug or tags. Show what does exist.
- **Nothing at all** → say so plainly. Don't invent a starting point.
- **`--latest` returned something days old, or several handoffs share the newest date** →
  name what you picked and offer the alternatives, rather than silently assuming.

## Step 2 — Read it, then check it against reality

Read the document fully. Then spend a moment confirming the world hasn't moved underneath
it, because a handoff is a snapshot and the gap since it was written is exactly where
wrong assumptions live. Someone may have merged the branch, or the user may have kept
working without you.

Check what the document actually claims — typically:

- the branch it names: does it exist, is it checked out, has it moved, is it pushed?
- files and paths it references: still there?
- an MR or ticket it names: still open, still in the state described?

Keep this proportionate — a handful of quick reads, not an investigation. If something has
changed, say so up front and adjust the plan; don't follow stale instructions off a cliff.

## Step 3 — Load the suggested skills

Invoke the skills listed in the document's **Suggested skills** section before starting, so
their guidance is in context while you work rather than recalled halfway through.

## Step 4 — Confirm the plan, then work

Give the user a short orientation — a few lines, not a re-read of the document:

- which handoff you loaded (id and title)
- where things stand, and anything you found that has changed since
- what you're about to do first
- any open questions the document flagged for them

Then do the work. Read **Already tried — don't redo** before you start on anything: it
exists precisely because those dead ends are invisible from the code and cost the previous
session real time.

If the document's open questions block the first step, ask them now rather than picking a
direction and discovering later that it was the wrong one.

## When the session ends

If the work still isn't finished, run `/handoff` — and tell it to **update this same
document** rather than write a new one. One identifier following a thread of work across
sessions stays navigable; a pile of near-duplicate handoffs about the same task does not.
