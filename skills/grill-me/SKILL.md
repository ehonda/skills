---
name: grill-me
description: A relentless interview to sharpen a plan or design, followed by a spec for review. Invoke it only when the user asks for a grilling; never reach for it on your own.
---

Invoke `grilling` and complete the interview, including the user's confirmation that you have reached a shared understanding. Do not implement the design.

For the spec, use an explicit target repository and worktree if the user gave one. Otherwise, use the current branch and worktree only if they were designated for this grilling. If neither is clear, ask the user where to write it.

The **grilling record** is the interview's evidence, for whoever later checks the spec against what the user decided. Decide the spec's path first, following the repository's conventions, and write the record beside it as `<spec-name>.grilling.md`. Give one entry per question, in the order asked and grouped by round: the question and its options as asked, your recommendation, the user's answer verbatim (never paraphrased), and the decision that resulted. Where looked-up facts settled a question, note briefly what you found.

Then write the grilling record in the target worktree, invoke `to-spec`, write the spec from the interview there following that repository's conventions, and present both for review.
