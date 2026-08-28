# Attribution

Skills in this repository that were written by someone else, with what this layer changed about
them. Generated from [`vendor/manifest.yaml`](./vendor/manifest.yaml); the "changed here" column is
derived by comparing each pristine copy under `vendor/` with its shipped copy under `skills/`, so
it cannot go stale.

Everything not listed here is my own work, under [LICENSE](./LICENSE).

## From [mattpocock/skills](https://github.com/mattpocock/skills)

Copyright (c) 2026 Matt Pocock, MIT. Their licence is kept verbatim at
[`vendor/mattpocock/LICENSE`](./vendor/mattpocock/LICENSE) and covers the vendored skills below,
both the pristine copies and this layer's modified versions of them.

Vendored at commit [`5b15a47`](https://github.com/mattpocock/skills/tree/5b15a47f2d7150f545fbcacbfe381787fc0230dc) on 2026-08-29.

| Skill | Changed here |
|---|---|
| [grilling](./skills/grilling) | No, vendored verbatim. |
| [grill-me](./skills/grill-me) | Yes. Made model-invocable (dropped `disable-model-invocation` and `allow_implicit_invocation: false`), with the restraint moved into the description instead, because a user-invoked skill cannot be reached mid-message. |
| [grill-with-docs](./skills/grill-with-docs) | Yes. Same change as `grill-me`, for the same reason. |
| [domain-modeling](./skills/domain-modeling) | Yes. Added a section on resolving which repository `CONTEXT.md` and `docs/adr/` belong to, because a session started above or beside a repository would write them into the wrong one. |

To see any of these exactly, without trusting this table:

```bash
git diff --no-index vendor/mattpocock/<skill> skills/<skill>
```

## Owed a mention, but not vendored

`handoff` began as a reading of Matt Pocock's
[`handoff`](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md)
and kept its shape: a document written for the next agent, referencing artifacts instead of
restating them, redacting secrets, steered by the arguments you pass. Almost none of his text
survived. His is sixteen lines; this one is ninety-one, and the only lines the two share are the
frontmatter fence and `name: handoff`.

It is therefore listed here as an acknowledgement rather than in
[`vendor/manifest.yaml`](./vendor/manifest.yaml), and it carries no pristine copy: a base that
shares no wording with the shipped skill would make every diff and every merge against it
meaningless. `handoff-resume` is wholly my own, with no upstream equivalent.

