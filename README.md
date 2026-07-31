# jabroni-skills
Yet another Claude skills repo. Weow.

Personal Claude Code plugin marketplace. One plugin (`jabroni`) bundles all skills under `plugins/jabroni/skills/`.

## Install (local)

Inside a `claude` session:

```
/plugin marketplace add /Users/vio/Documents/source/jabroni-skills
/plugin install jabroni@jabroni-skills
```

From a shell, outside a session:

```
claude plugin marketplace add /Users/vio/Documents/source/jabroni-skills
claude plugin install jabroni@jabroni-skills
```

## Add a skill

1. `plugins/jabroni/skills/<skill-name>/SKILL.md` with `name` + `description` frontmatter.
2. Optional `references/`, `scripts/` next to it.
3. `/plugin marketplace update jabroni-skills` (or reinstall) to pick up changes.

## Layout

```
.claude-plugin/marketplace.json   # marketplace manifest
plugins/jabroni/
  .claude-plugin/plugin.json      # plugin manifest
  skills/<skill-name>/SKILL.md    # one folder per skill
```
