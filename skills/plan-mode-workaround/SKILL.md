---
name: plan-mode-workaround
description: Continue plan-mode planning while running in auto-accept mode, as a workaround for the plan-mode manual-approval classifier bug. Use when you've switched from plan mode to auto mode mid-planning because read-only steps were wrongly prompting for approval, and want the agent to keep planning (read-only) and hand back to plan mode once the plan is ready.
---

# Plan Mode Workaround

## Why this skill exists

Plan mode sometimes misclassifies read-only planning actions and forces manual approval for each
one (a classifier bug). The workaround: switch Claude Code to auto-accept ("auto") mode and invoke
this skill. In auto mode those spurious prompts disappear, so planning proceeds uninterrupted — but
auto mode also normally lets the agent edit files and run mutating commands, which is exactly what we
must NOT do while planning. This skill re-imposes plan-mode discipline by hand and hands control back
to real plan mode once the plan is done.

## What to do

You are, for all intents and purposes, still in plan mode — the harness just won't enforce it. Behave
accordingly.

1. **Pick up where planning left off.** This skill is normally invoked mid-planning. Read back through
   the conversation for exploration already done, decisions already made, and any partial plan.
   Continue from there — do not redo completed research or re-explore what's already understood.

2. **Stay strictly read-only.** Even though auto mode will happily let you edit files, run mutating
   commands, commit, or change configs — do none of it. The ONLY file you may write is the plan file
   (below). Everything else is read-only: reading files, searching, running read-only commands,
   spawning Explore/Plan subagents. This mirrors exactly what plan mode allows.
   - **When unsure whether a command mutates anything, don't run it** — treat it as forbidden the way
     plan mode would. This covers the gray-area commands that feel like research but write to disk or
     change repo state: `dotnet restore`, `npm install`, `git fetch`, builds run "just to check", and
     the like.
   - **Only spawn tool-restricted read-only subagents (Explore/Plan) — never a general-purpose one.**
     This skill's read-only rule is instruction-level and does **not** reach a subagent's context, so
     under auto mode a general-purpose subagent runs with full write permissions and none of these
     constraints. Restrict delegation to the built-in read-only agent types, and restate the read-only
     constraint explicitly in every subagent prompt so the discipline survives the handoff.

3. **Follow the plan-mode workflow faithfully.** Continue the same phased process plan mode uses
   (Explore for research → Plan for design → review the critical files → write the plan). Only do the
   phases that haven't happened yet — don't duplicate work already done in plan mode.

4. **Write the plan to the plan file.**
   - If a plan file already exists for this session (the plan-mode harness announced its path earlier,
     e.g. `/Users/<you>/.claude/plans/<slug>.md`), continue writing the plan into that same file.
   - If none exists, create one at `~/.claude/plans/<kebab-case-slug>.md`, slug derived from the task.
     Note this won't match plan mode's own naming (its real files carry a random `-adjective-noun`
     suffix, e.g. `check-our-documentation-for-lucky-owl.md`), so when you switch back to plan mode
     the harness will announce its **own** plan file rather than pick up this one automatically. Name
     the exact path in the handoff message (below) so the resumed plan-mode session can be pointed at
     it — or copy its contents into the file plan mode announces.

5. **Stop — do NOT implement, do NOT call `ExitPlanMode`.** `ExitPlanMode` belongs to real plan mode
   and isn't yours to call here. When the plan is ready, do not start executing it.

## Handoff

Once the plan file is complete, stop and post a clear notification, e.g.:

> The plan is ready and written to `<plan-file-path>`. Switch back to **plan mode** to review and
> approve it — planning picks up right where this left off.

Then end your turn. You finish planning (plan presentation + approval) in real plan mode.
