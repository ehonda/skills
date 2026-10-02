---
name: grill-with-docs
description: A relentless interview that records glossary and ADR decisions, then writes a spec for review. Invoke it only when the user asks for a grilling; never reach for it on your own.
---

Invoke `grilling` and `domain-modeling` for the interview. Do not implement the design.

Use an explicit target repository and worktree if the user gave one. Otherwise, use the current branch and worktree only if they were designated for this grilling. If neither is clear, ask the user before changing files.

Apply resolved glossary and ADR changes directly in that worktree as the interview proceeds, following `domain-modeling`. Do not defer those file changes to the spec or list them as work to implement. The spec can refer to the decisions where relevant.

The **grilling record** is the interview's evidence, for whoever later checks the spec against what the user decided. Decide the spec's path first, following the repository's conventions, and write the record beside it as `<spec-name>.grilling.md`. Give one entry per question, in the order asked and grouped by round: the question and its options as asked, your recommendation, the user's answer verbatim (never paraphrased), and the decision that resulted. When a decision changed a glossary or ADR file, name the file. Where looked-up facts settled a question, note briefly what you found.

After the user confirms that you have reached a shared understanding, write the grilling record in the target worktree, invoke `to-spec`, write the spec from the interview there following that repository's conventions, and present both for review.
