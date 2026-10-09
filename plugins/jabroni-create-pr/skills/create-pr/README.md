# Create PR Skill

A Claude Code skill for drafting and opening GitHub pull requests with a structured, why-first body.

## Usage

```
/create-pr
```

Also auto-triggers on natural language ("open a PR for this", "create a pull request"). Claude asks for the missing pieces, drafts the PR body, shows it to you, and only opens the PR once you approve.

## What it does

1. Checks the current branch, upstream state, and whether an open PR already exists for it
2. Checks whether the target repo has a GitHub PR template (`.github/pull_request_template.md`) — if missing, adds one as its own commit, kept separate from any feature-work changes
3. Asks for the PR title, the *why* behind the change (Purpose), where the work comes from (Context), and any extra materials (Notes) — never guesses these
4. Fills in the template and shows you the full drafted title and body
5. On approval: commits the template file if needed, pushes the branch if it has no upstream, then creates the PR via `gh-axi pr create`

## PR template

```markdown
# Purpose :dart:

Why these changes are happening — not what changed, your code already shows that.

# Context :brain:

Where the work comes from — an issue, a Trello card, or "spotted while working on X". Link a prior PR if this follows from one.

# Notes :notebook:

Extra materials for reviewers — references, screenshots, anything useful.
```

| Section | What goes there |
|---------|-----------------|
| Purpose | The *why* — motivation, problem solved, benefit. Never the what. |
| Context | Origin of the work — issue/Trello link, "unprompted", or a linked prior PR |
| Notes | Optional — references, screenshots, supporting materials |

All prose is written in Simple Technical English (ASD-STE100).

## Hard limits

The skill will **never**:

- Guess or infer Purpose/Context — it always asks, and pushes back on thin answers
- Fabricate a Trello/issue link you didn't give it
- Force-push, in any form
- Create the PR without showing you the full draft and getting explicit approval
- Bundle the PR-template commit with feature-work changes

## Why why-first?

Same reasoning as the [commit skill](../commit/README.md): the diff already shows what changed. A reviewer without your context needs the *why* to review well, and future readers need it to understand the change's intent.

## Files

- `SKILL.md` — machine-readable instructions loaded into Claude's context
- `references/template.md` — the verbatim PR template; also written into a target repo's `.github/pull_request_template.md` when missing
- `README.md` — this file
