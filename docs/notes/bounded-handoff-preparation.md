# Why handoff preparation is bounded

Date: 2026-10-07

The `handoff` skill now treats writing a handoff as a bounded checkpoint of established
state. This note records the observations and reasoning behind that change.

## Observations

Local Codex session traces showed that short handoff requests could take several minutes
without producing a document. File reads, writes and index refreshes completed quickly;
most elapsed time occurred between tool calls, while the model continued processing.
Some traces contained repeated reasoning items, and one completed request recorded
substantial reasoning after a draft already existed.

After an interruption asking why an update was taking so long, the model said it had
"overthought it" and completed the same request much sooner. That self-report is
consistent with the trace, but is not a complete explanation of every delay. Reasoning
contents were not available for inspection, and some slow requests also encountered
stream disconnections or timeouts.

The available Claude Code examples did not show the same extreme delays. These were
observational samples, not a controlled comparison of clients or models. Large
conversation contexts, reasoning settings and transport behavior could also affect
latency. The evidence does not establish a single Codex defect or a guaranteed speedup.

## Change and rationale

The previous instructions emphasized a useful, complete continuation document without
an explicit stopping point for gathering evidence or refining the draft. The working
hypothesis is that this left room for the model to reconsider established state or
reconstruct more history than the next session needed.

The shared workflow therefore asks the writer to:

- Use evidence already established in the conversation and reference existing artifacts.
- Check a source narrowly when a required fact is missing, or record the unknown and the
  check the next session must make.
- Write once the next action, current state, references and constraints are clear.
- Verify the saved document and index once, revising further only for a specific defect.

Variants inherit these bounds while retaining their required checks and authorization
decisions. This changes how preparation ends, without attempting to change model effort
settings or fix transport failures.

## Why updates remain

Slow requests included both fresh documents and updates, so replacing every update with
a new document would not address the common behavior. Stable identifiers also make a
continuing thread easier to find.

Updates can become unnecessarily expensive if the document accumulates chronological
session summaries or repeatedly carries artifact contents forward. The revised workflow
replaces stale state and continuation steps, retains relevant context and links durable
artifacts. It does not archive the whole previous document unless the user asks.

The expected benefit is less unnecessary preparation. Future usage must establish
whether latency improves while handoffs still support a correct next action.
