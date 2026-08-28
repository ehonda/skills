# skills

My agent skills. Some I wrote, some I took from other people and changed a little, and the
difference is recorded rather than blurred: see [ATTRIBUTION.md](./ATTRIBUTION.md).

They work with Claude Code, Codex, and anything else the
[`skills` CLI](https://github.com/vercel-labs/skills) knows about.

## Install

```bash
npx skills add ehonda/skills -g -y          # every skill, for every agent you have
npx skills add ehonda/skills -g -y --skill grilling   # just one
```

## What's here

| Skill | What it does |
|---|---|
| [grilling](./skills/grilling) | Interviews you about a plan until every branch of the design tree is resolved. |
| [grill-me](./skills/grill-me) | The grilling interview on its own. |
| [grill-with-docs](./skills/grill-with-docs) | The same interview, writing the glossary and ADRs as decisions land. |
| [domain-modeling](./skills/domain-modeling) | Builds and sharpens a project's domain model. |

All four come from [mattpocock/skills](https://github.com/mattpocock/skills). Three of them are
changed here; the changes and the reasons are in [ATTRIBUTION.md](./ATTRIBUTION.md).

## How this repository works

It is a **layer**: it ships skills, and the vendored ones keep a byte-exact pristine copy of what
they looked like in their source, so the customization is a diff anyone can read:

```bash
git diff --no-index vendor/mattpocock/grill-me skills/grill-me
```

That means updates from the source can still be merged three-way, without this being a fork of
anyone's repository. The machinery lives in
[`ehonda/skill-vendor`](https://github.com/ehonda/skill-vendor), which also holds the glossary and
the decisions behind the model.

## Licence

My own work is MIT, [LICENSE](./LICENSE). Vendored skills stay under their source's licence, kept
verbatim beside the pristine copies, and are listed in [ATTRIBUTION.md](./ATTRIBUTION.md).
