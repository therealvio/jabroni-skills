# jabroni-skills
Yet another Claude skills repo. Weow.

Personal Claude Code plugin marketplace. One plugin (`jabroni-contraptions`) bundles all skills under `plugins/jabroni-contraptions/skills/`.

## Install

Inside a `claude` session:

```
/plugin marketplace add therealvio/jabroni-skills
/plugin install jabroni-contraptions@jabroni-skills
```

From a shell, outside a session:

```
claude plugin marketplace add therealvio/jabroni-skills
claude plugin install jabroni-contraptions@jabroni-skills
```

## Add a skill

1. `plugins/jabroni-contraptions/skills/<skill-name>/SKILL.md` with `name` + `description` frontmatter.
2. Optional `references/`, `scripts/` next to it.
3. `/plugin marketplace update jabroni-skills` (or reinstall) to pick up changes.

## Layout

```
.claude-plugin/marketplace.json   # marketplace manifest
plugins/jabroni-contraptions/
  .claude-plugin/plugin.json      # plugin manifest
  skills/<skill-name>/SKILL.md    # one folder per skill
```
