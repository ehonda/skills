---
name: grill-me
description: A relentless interview to sharpen a plan or design, followed by a spec for review. Invoke it only when the user asks for a grilling; never reach for it on your own.
---

Invoke `grilling` and complete the interview, including the user's confirmation that you have reached a shared understanding. Do not implement the design.

For the spec, use an explicit target repository and worktree if the user gave one. Otherwise, use the current branch and worktree only if they were designated for this grilling. If neither is clear, ask the user where to write it.

Then invoke `to-spec`, write the spec from the interview in the target worktree following that repository's conventions, and present it for review.
