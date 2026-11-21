---
name: create-pr
description: Draft and create a GitHub PR via gh-axi, using the repo's own PR template when it has one, else the bundled Purpose/Context/Notes template. Use when asked to open a PR, create a pull request, draft a PR, or push a PR for the current branch.
disable-model-invocation: false
---

TEMPLATE PRIORITY: the target repo's own PR template wins. Use the bundled template only when the repo has none.

REPO TEMPLATE (found in step 2, in the repo or in the owner's `.github` repository):
- Keep its headings, order, and checklists exactly. Never add, remove, or rename sections.
- Read each section's HTML comments (`<!-- -->`) as instructions for what to ask and write. Remove the comments from the final body.
- Checklist items (`- [ ]`): tick only items the user confirms. Never tick on their behalf.
- A section with no answer stays, with "N/A" under its heading, unless the template says it is optional.

BUNDLED TEMPLATE: `references/template.md` — three sections, exact headings/emoji, never alter structure:
- Purpose :dart: — WHY the change happened, not what (code shows what)
- Context :brain: — origin of the work: issue/Trello link, "spotted while working on X", or link to a prior PR this follows from
- Notes :notebook: — optional extra materials (references, screenshots)

All prose in ASD-STE100 (Simplified Technical English). GitHub alerts (`> [!NOTE]`, `> [!WARNING]`, `> [!IMPORTANT]`, `> [!CAUTION]`, `> [!TIP]`) allowed inside sections for emphasis.

FORBIDDEN — refuse unconditionally, no alternatives:
- Infer or guess Purpose/Context — always ask; never fabricate a Trello/issue link the user didn't give
- Force-push (any form)
- Create the PR without showing full drafted title + body and getting explicit approval first
- Add the bundled template to a repo that already has a PR template, or that gets one from the owner's `.github` repository, or swap the repo template for the bundled one
- Bundle the template-add commit with feature-work changes — stage it by exact path only, never `-A` or `.`
- Hard-wrap PR body text. Write each paragraph and each bullet on one line, however long. GitHub reflows the text. (Commit messages are different: they still wrap at 72 columns.)
- Add a "Generated with Claude Code" footer, or any other tool attribution, to the PR body. The body ends after the last template section.

WORKFLOW:
1. Pre-flight: `git status`, `git branch -vv` → current branch, upstream state, staged/unstaged changes. `gh-axi pr list --head <branch>` → warn if branch already has an open PR.
2. Template lookup. Match file names case-insensitively, `.md` or `.txt`. Check all of these places in the target repo, from the repo root (`git rev-parse --show-toplevel`):
   - `pull_request_template` in `.github/`, the repo root, or `docs/`
   - any file in `.github/PULL_REQUEST_TEMPLATE/`, `PULL_REQUEST_TEMPLATE/`, or `docs/PULL_REQUEST_TEMPLATE/`
   Then:
   - Exactly one template → use it (see REPO TEMPLATE). Tell the user which file you use.
   - More than one, in one place or across places → list every path and ask which one to use. Never pick one yourself.
   - None → check the owner's `.github` repository, which GitHub uses as the default for repos with no template of their own. Get the owner with `gh-axi api 'repos/{owner}/{repo}' --jq .owner.login`, then check the same places with `gh-axi api repos/<owner>/.github/contents/<dir>` (`<dir>` empty for the root). A 404 means no such repo or folder. Apply the same one/many rules. Tell the user the template comes from `<owner>/.github`.
   - None in either → use the bundled template. Note that `references/template.md` content goes to `.github/pull_request_template.md` as its own commit later. Never create this file mid-step; fold the action into the confirmation in step 5.
3. Gather info — ask, never infer (see CONTEXT-GATHERING). Need: title, an answer for each template section (Notes or other optional sections may be "none"), base branch if ambiguous.
4. Draft: fill the template sections from gathered answers, ASD-STE100 prose, structure untouched. One line per paragraph and per bullet; no manual line breaks inside them.
5. Confirm: show exact title + full body, plus any pending actions (template commit, branch push) as a single list. Long lines in the draft are expected; GitHub reflows them. One explicit approval gates everything below.
6. Execute, in order: commit bundled template file if the repo had none (message e.g. `chore: add PR template`, path-only stage) → push branch if no upstream (`git push -u origin <branch>`, never force) → `gh-axi pr create --title "<title>" --body-file <tmpfile> --base <base>` → report PR URL. Delete tmpfile after.

CONTEXT-GATHERING:
- Repo template: ask one question per section, guided by its heading and comments. Apply the Purpose rule to any section that asks why, and the Context rule to any section that asks for an issue, ticket, or origin.
- Purpose: must state why, not what. Push back on thin answers ("fix bug" → "what bug? who's affected? what breaks without this?"). Keep asking until a reviewer with zero context would understand the motivation.
- Context: "unprompted, spotted while working on X" is a valid answer. A tracked item (issue/Trello) or a prior PR being followed up on requires a link — ask for it if missing.
- Notes: optional. "none" is fine.
- Base branch: never assume. Ask if repo default isn't obviously main/master or user hasn't said.

EDGE CASES:
- No commits yet on branch → warn, let user decide whether to proceed
- Branch already has an open PR → warn with link, ask whether to proceed anyway or stop
- Current branch is main/master → warn strongly, don't refuse outright
- `gh-axi pr create` fails → surface the error verbatim, don't retry silently
