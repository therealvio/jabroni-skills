# jabroni-skills
Yet another Claude skills repo. Weow.

Personal Claude Code plugin marketplace. Each skill is its own plugin under `plugins/<skill-name>/`, so you install only what you need.

## Install

Inside a `claude` session:

```
/plugin marketplace add therealvio/jabroni-skills
/plugin install commit@jabroni-skills
/plugin install create-pr@jabroni-skills
/plugin install comment@jabroni-skills
```

From a shell, outside a session:

```
claude plugin marketplace add therealvio/jabroni-skills
claude plugin install commit@jabroni-skills
claude plugin install create-pr@jabroni-skills
claude plugin install comment@jabroni-skills
```

## Available plugins

| Plugin | Skill |
|--------|-------|
| `comment` | Add or improve code comments and docstrings using intent-first, language-native conventions |
| `commit` | Create git commits following Conventional Commits specification |
| `create-pr` | Draft and create a GitHub PR using the team template, via gh-axi |

## Add a skill

1. `plugins/<skill-name>/.claude-plugin/plugin.json` with `name` + `description`.
2. `plugins/<skill-name>/skills/<skill-name>/SKILL.md` with `name` + `description` frontmatter.
3. Optional `references/`, `scripts/` next to it.
4. Add the plugin to `.claude-plugin/marketplace.json`.
5. `/plugin marketplace update jabroni-skills` (or reinstall) to pick up changes.

## Layout

```
.claude-plugin/marketplace.json       # marketplace manifest, lists all plugins
plugins/<skill-name>/
  .claude-plugin/plugin.json          # plugin manifest
  skills/<skill-name>/SKILL.md        # the skill itself
```
