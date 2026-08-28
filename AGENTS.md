# Conventions

## Skills

Every skill lives at `skills/<name>/SKILL.md`, flat, with the directory name matching the
frontmatter `name:`. The `skills` CLI walks `skills/` three levels deep, so nothing that is not a
shippable skill may live under it. In a layer, that is why pristine copies sit in `vendor/`.

### Invocability is declared twice, and must agree

Claude Code reads YAML frontmatter in `SKILL.md`; Codex reads `agents/openai.yaml` beside it. The
two spellings are inverted, which is exactly why they drift:

| | user-invoked (only when typed) | model-invoked (typed or reached for) |
|---|---|---|
| `SKILL.md` frontmatter | `disable-model-invocation: true` | omit the key |
| `agents/openai.yaml` | `policy.allow_implicit_invocation: false` | omit the `policy` block |

**Every skill carries both files.** Changing invocability means changing both, in the same commit.

## Prose

No em-dashes anywhere in this repository. Where a sentence reaches for one, rewrite it with a
comma, colon, period, parentheses or a conjunction, whichever the sentence actually wants. Never
substitute the character blindly.

## Documentation

- `CONTEXT.md` is a glossary and nothing else. No implementation detail.
- `docs/adr/NNNN-slug.md` records decisions that are hard to reverse, surprising without context,
  and the result of a real trade-off. If a decision is none of those, it does not get an ADR.

## This repository is a layer

Vendored skills exist twice: the pristine copy under `vendor/<source>/<name>/` and the shipped skill
under `skills/<name>/`. **Never edit anything under `vendor/`.** It is the base a refresh merges
against, so editing it destroys the record of what this layer changed.

Adding, changing or refreshing a vendored skill is what
[`ehonda/skill-vendor`](https://github.com/ehonda/skill-vendor) is for. Do not do it by hand:
provenance and attribution have to move with the files.
