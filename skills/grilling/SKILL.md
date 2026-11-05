---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, each question with your recommended answer.

Each round is a gated Plannotator annotate round on a **round document** (the `plannotator` tool when you have one, otherwise the CLI; the `plannotator` skill owns the mechanics, including how to wait for the decision). Do not ask the questions in chat.

The round document is one Markdown file for the whole interview. Use the path the caller names, if one does. Otherwise write it to `~/.agents/grillings/<slug>.md`, outside every repository. Keep the same path in every round, so the user sees what changed since the last one.

Lay it out like so:

```markdown
# Grilling: <topic>

## Settled

- **<decision title>**: <the decision, in one or two sentences>

## Round 2

:::question
<question>

<context: what the choice affects, what you know, why you recommend it>

- [ ] <choice>
- [ ] <choice>

Recommended: <choice>
:::
```

Write one question block per frontier question, following the `plannotator` skill's rules for question blocks: context lines under the question, `:::question-multi` when several choices can hold, `:::question-text` when the answer is open. If the frontier is larger than that skill's per-round limit, ask the questions that unblock the most of the tree first; the rest wait for the next round.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. On feedback, take an annotation on a question as part of its answer, and treat pushback as an answer too, not as noise. An annotation on a **Settled** decision reopens it: move it back as a question in the next round, and an annotated final round is not approved until that question is answered. Before you drop an answered block, append it to a scratch file beside the round document (`<round document name>.record.md`): the question and its options as asked, your recommendation, and the answer verbatim as Plannotator returned it, with any annotations. Keep that file until the grilling record is written, since the round document keeps only the decision. Then move every answered question to **Settled** as a decision and drop its block. A skipped question keeps its block. Then recompute the frontier, replace the round section with the next round, and open it on the same path. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one. If a round is dismissed without a decision, stop and ask the user in chat how to continue.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; open the round with the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Then open a final round with no question blocks and only **Settled**, and say in a line above it that approving confirms the shared understanding. Approving an earlier round does not: a plain approval may carry no answers, so a question block still in the document counts as unanswered and goes into the next round. Do not act on the interview until the user approves the final round.
