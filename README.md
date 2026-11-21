# jabroni-skills
Yet another agent skills repo. Weow.

These are personal curated skills. Feel free to use, contribute, or fork, I don't mind!

The skills work in [Claude Code](https://claude.com/claude-code) and [Pi](https://pi.dev). Same `SKILL.md` files, two ways to install.

## Claude Code

Each skill is its own plugin, so you take what you need.

Inside a `claude` session:

```
/plugin marketplace add therealvio/jabroni-skills
/plugin install jabroni-commit@jabroni-skills
/plugin install jabroni-create-pr@jabroni-skills
```

From a shell, outside a session:

```
claude plugin marketplace add therealvio/jabroni-skills
claude plugin install jabroni-commit@jabroni-skills
claude plugin install jabroni-create-pr@jabroni-skills
```

Run a skill with `/<plugin>:<skill>`, for example `/jabroni-commit:commit`.

## Pi

The repo is one Pi package that holds all the skills.

```
pi install git:github.com/therealvio/jabroni-skills
```

- Pin a version: append `@<tag>` to the source.
- Install for one project only: add `-l` (writes `.pi/settings.json` instead of `~/.pi/agent/settings.json`).
- Try it without installing: `pi -e git:github.com/therealvio/jabroni-skills`, or `pi -e ./` from a clone.

### Load only some skills

By default you get every skill. To pick, install as above, then open the settings file (`~/.pi/agent/settings.json`, or `.pi/settings.json` with `-l`). `pi install` adds the package as a plain string:

```json
{
  "packages": ["git:github.com/therealvio/jabroni-skills"]
}
```

Change that string to an object with a `skills` filter. Paths are relative to the repo root.

Only the `commit` skill:

```json
{
  "packages": [
    {
      "source": "git:github.com/therealvio/jabroni-skills",
      "skills": ["plugins/jabroni-commit/skills/*"]
    }
  ]
}
```

Everything except `create-pr`:

```json
{
  "packages": [
    {
      "source": "git:github.com/therealvio/jabroni-skills",
      "skills": ["!plugins/jabroni-create-pr/**"]
    }
  ]
}
```

To take more skills, add more paths to `skills`. Restart Pi or run `/reload` to apply.

Manage it:

```
pi update --extensions                              # update all packages
pi list                                             # show configured packages
pi remove git:github.com/therealvio/jabroni-skills  # remove
```

Run a skill with `/skill:<name>`, for example `/skill:commit`.

## Available skills

| Skill       | Claude plugin       | Description                                                                           |
| ----------- | ------------------- | ------------------------------------------------------------------------------------- |
| `commit`    | `jabroni-commit`    | Create git commits following Conventional Commits specification                       |
| `create-pr` | `jabroni-create-pr` | Draft and create a GitHub PR using the repo template or a bundled default, via gh-axi |

`create-pr` needs `gh-axi` on your `PATH`, in either harness.

## Add a skill

1. `plugins/jabroni-<skill-name>/.claude-plugin/plugin.json` with `name` (`jabroni-<skill-name>`) + `description`.
2. `plugins/jabroni-<skill-name>/skills/<skill-name>/SKILL.md` with `name` + `description` frontmatter.
3. Optional `references/`, `scripts/` next to it.
4. Add the plugin to `.claude-plugin/marketplace.json`.
5. `/plugin marketplace update jabroni-skills` (or reinstall) to pick up changes.

Pi needs no manifest change. It finds every skill under `plugins/*/skills`.

## Layout

```
package.json                          # Pi package manifest (`pi` field)
.claude-plugin/marketplace.json       # Claude marketplace manifest, lists all plugins
plugins/jabroni-<skill-name>/
  .claude-plugin/plugin.json          # Claude plugin manifest
  skills/<skill-name>/SKILL.md        # the skill itself
```
