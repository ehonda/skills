---
name: elaborate-decisions
description: Expand the decisions you left open for the user into a document they can decide from, then settle them in Plannotator question rounds. Use when the user asks you to elaborate on pending decisions or questions so they can decide.
argument-hint: "[which decisions, if not all the open ones]"
---

# Elaborate Decisions

You left the user one or more decisions, each squeezed into a line or two, and they asked for more
before they choose. Turn each into something they can decide from cold, then settle all of them in
one Plannotator document.

## Which decisions

By default, every decision your last message left open. If the user names some (by number, title or
quote), take only those. A decision is something that changes what you do next and that you cannot
settle from the code, the docs or what the user already said.

## 1. Gather before you write

Re-read what each decision rests on: the review thread, the spec section, the code, the earlier
proposal. You compressed it once; expanding from memory repeats whatever the compression lost.

While you read:

- **Check how existing code or conventions already handle it.** If the system already behaves like
  one option, that is the most useful sentence in the elaboration. The user will ask otherwise.
- **Check your own numbers and claims.** If the earlier summary was wrong, say so and correct it.
- **Drop decisions that are not the user's.** If one answer follows directly from a goal the user
  already set, it was never a decision. Say which way it goes and why, take it out of the round, and
  report it in the document under `## Not in this round` (step 2) so the user can veto the call.

Finding facts is your job. Dispatch a sub-agent for a lookup that would take more than a few calls.

## 2. Write the document

One Markdown file for the whole elaboration, at `~/.agents/elaborations/<yyyy-mm-dd>-<slug>.md`,
outside every repository. Keep the path for every round.

Open with one or two sentences that frame the set: what these decisions have in common, whether the
normal path is already covered and only edge cases remain, whether they are independent of each
other. If you dropped any decision in step 1, follow with a `## Not in this round` list: one line per
dropped decision, saying which way it goes and why. The user can veto a call by annotating it, and
you then restore it as a section. Then one section per decision:

```markdown
## 1. <the decision, phrased as a plain question>

<The situation: what the system does today and where the gap is, in plain words.>

<A concrete scenario: "Suppose ... Later ..." with named placeholders (X, Y, activation A) for the
things involved. This is what makes the abstract choice decidable.>

| Option | What it means |
|---|---|
| **A: <short name>** | <what changes, what it costs, what it enables> |
| **B: <short name>** | <same> |

<What neither option does, when that clears up a likely worry: "Neither option cancels work that
already ran.">

**Recommendation: A.** <Why, in terms the user weighs: how often the case occurs, effort now versus
later, what stays reversible. If a reviewer or an earlier step recommended differently, say why.>

[Source](<link to the thread, spec section or file the decision came from>)

:::question
<The same question, one line>

<One to three sentences of context, for someone reading only this block.>

- [ ] A: <short name>
- [ ] B: <short name>

Recommended: A: <short name>
:::
```

Follow the `plannotator` skill's rules for question blocks: `:::question-multi` when several choices
can hold together, `:::question-text` when the answer is open. Use a table only when the options
differ along more than one line each; two short options read fine as a list. Keep it plain: the user
is switching in from other work, so name things by what they do, not by internal identifiers, and
introduce any term the decision depends on.

## 3. Settle it in rounds

Open the file as a gated Plannotator annotate round (the `plannotator` tool when you have one,
otherwise the CLI; the `plannotator` skill owns the mechanics and how to wait for the decision). In
chat, say only that the round is open and where the file is. Do not repeat the elaboration there.

When the round comes back:

- An answered question is settled, together with any note on it. Record each settled decision under
  a `## Settled` section at the top (the decision and the user's reason, if they gave one) and drop
  its section.
- An annotation that asks something ("how does X handle this today?") is not an answer. Find out,
  rewrite that decision's section with what you learned, keep its question block, and say at the top
  of the section what changed.
- Pushback on a recommendation or on the framing is an answer too, or a reason to rewrite the
  section. It is not noise.
- An `Other: ...` answer (free text instead of a listed choice) is the user's decision: record it
  under Settled. If it reads as a question, treat it as the annotation case above.
- A skipped question keeps its block.

Open the next round on the same path while any question remains. When none does, open a final round
with only **Settled** (and **Not in this round**, if any) and a line above it saying that approving
confirms these decisions. A plain approval of a round that still has question blocks does not
settle them. If a round is dismissed without a decision, stop and ask in chat how to continue.

## 4. Hand the answers back

After the final approval, list the decisions in chat, one line each, and add any dropped decisions
as calls you made that the user did not veto. Then continue with whatever was waiting on them,
within the authority that work already had. Recording the decisions where they came from (a review
thread, a spec, a ticket) belongs to that work, not to this skill, unless the user asks.
