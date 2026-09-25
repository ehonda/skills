---
name: grill-with-docs
description: A relentless interview that records glossary and ADR decisions, then writes a spec for review. Invoke it only when the user asks for a grilling; never reach for it on your own.
---

Invoke `grilling` and `domain-modeling` for the interview. Do not implement the design.

Use an explicit target repository and worktree if the user gave one. Otherwise, use the current branch and worktree only if they were designated for this grilling. If neither is clear, ask the user before changing files.

Apply resolved glossary and ADR changes directly in that worktree as the interview proceeds, following `domain-modeling`. Do not defer those file changes to the spec or list them as work to implement. The spec can refer to the decisions where relevant.

After the user confirms that you have reached a shared understanding, invoke `to-spec`, write the spec from the interview in the target worktree following that repository's conventions, and present it for review.
