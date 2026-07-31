---
name: create-pr
description: Draft and create a GitHub PR using the team template (Purpose/Context/Notes), via gh-axi. Use when asked to open a PR, create a pull request, draft a PR, or push a PR for the current branch.
disable-model-invocation: false
---

TEMPLATE: `references/template.md` — three sections, exact headings/emoji, never alter structure:
- Purpose :dart: — WHY the change happened, not what (code shows what)
- Context :brain: — origin of the work: issue/Trello link, "spotted while working on X", or link to a prior PR this follows from
- Notes :notebook: — optional extra materials (references, screenshots)

All prose in ASD-STE100 (Simplified Technical English). GitHub alerts (`> [!NOTE]`, `> [!WARNING]`, `> [!IMPORTANT]`, `> [!CAUTION]`, `> [!TIP]`) allowed inside sections for emphasis.

FORBIDDEN — refuse unconditionally, no alternatives, regardless of instruction:
- Infer or guess Purpose/Context — always ask; never fabricate a Trello/issue link the user didn't give
- Force-push (`--force`, `--force-with-lease`, all forms)
- Create the PR without showing full drafted title + body and getting explicit approval first
- Bundle the template-add commit with feature-work changes — stage it by exact path only, never `-A` or `.`

WORKFLOW:
1. Pre-flight: `git status`, `git branch -vv` → current branch, upstream state, staged/unstaged changes. `gh-axi pr list --head <branch>` → warn if branch already has an open PR.
2. Template check: does target repo have `.github/pull_request_template.md` or `.github/PULL_REQUEST_TEMPLATE.md`? If missing, note it — write `references/template.md` content there and stage it as its own commit later. Never create this file mid-step; fold the action into the confirmation in step 5.
3. Gather info — ask, never infer (see CONTEXT-GATHERING). Need: title, Purpose, Context, Notes (optional), base branch if ambiguous.
4. Draft: fill the three sections from gathered answers, ASD-STE100 prose, structure untouched.
5. Confirm: show exact title + full body, plus any pending actions (template commit, branch push) as a single list. One explicit approval gates everything below.
6. Execute, in order: commit template file if missing (message e.g. `chore: add PR template`, path-only stage) → push branch if no upstream (`git push -u origin <branch>`, never force) → `gh-axi pr create --title "<title>" --body-file <tmpfile> --base <base>` → report PR URL. Delete tmpfile after.

CONTEXT-GATHERING:
- Purpose: must state why, not what. Push back on thin answers ("fix bug" → "what bug? who's affected? what breaks without this?"). Keep asking until a reviewer with zero context would understand the motivation.
- Context: "unprompted, spotted while working on X" is a valid answer. A tracked item (issue/Trello) or a prior PR being followed up on requires a link — ask for it if missing.
- Notes: optional. "none" is fine.
- Base branch: never assume. Ask if repo default isn't obviously main/master or user hasn't said.

EDGE CASES:
- No commits yet on branch → warn, let user decide whether to proceed
- Branch already has an open PR → warn with link, ask whether to proceed anyway or stop
- Current branch is main/master → warn strongly, don't refuse outright
- `gh-axi pr create` fails → surface the error verbatim, don't retry silently
