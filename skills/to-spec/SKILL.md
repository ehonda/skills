---
name: to-spec
description: "Turn the current conversation into a spec for the user to review. Invoke when the user asks for a spec or a grilling variant calls for one."
---

This skill takes the current conversation context and codebase understanding and produces a spec. Do NOT interview the user; just synthesize what you already know.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the spec, and respect any ADRs in the area you're touching.

2. Sketch out the seams at which you're going to test the feature. Existing seams should be preferred to new ones. Use the highest seam possible. If new seams are needed, propose them at the highest point you can. The fewer seams across the codebase, the better - the ideal number is one.

Do not stop to check the seams in chat. Write them into Testing Decisions; where you are unsure they match the user's expectations, ask there with a question block (see step 4).

3. Write the spec to a file using the template below. Use the path the grilling variant decided, if one did. Otherwise follow the repository's spec convention (for example `docs/specs/`), and if there is none, ask the user where to write it. Keep the same path in every round. If a grilling variant wrote a grilling record beside the spec, link it near the top of the spec, so a review of the spec can check it against what the user decided. That link is how the record is reviewed: do not open it as a separate round.

4. Present the spec for review in Plannotator, as a gated annotate round on the spec file (the `plannotator` tool when you have one, otherwise the CLI; the `plannotator` skill owns the mechanics). Ask every decision the conversation left open as a `:::question` block at the place in the spec it affects, following the `plannotator` skill's rules for question blocks, so the user answers them with a click in the same round as their annotations. Do not ask them in chat.

   On feedback, write each answer into the spec as a decision, drop its block, apply the annotations, and open the next round on the same path. A skipped question keeps its block. If a round is dismissed without a decision, stop and ask the user how to continue. On approval, replace each remaining question block with an entry under Further Notes that records the question as open. The spec is done when the user approves it.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>
