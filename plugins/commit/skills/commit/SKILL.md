---
name: commit
description: Create git commits following Conventional Commits specification. Explains why changes were made, not what changed. Use when committing code, writing a commit message, staging changes, or asked to "commit this".
disable-model-invocation: true
---

FORBIDDEN — refuse unconditionally, no alternatives:
- git push (any form), gh pr create, git reset/clean/restore/checkout (any form)
- Co-authored-by trailers attributing Claude
- git commit --amend unless user explicitly requests it (never infer from same-file/scope changes)
- Push/PR as default or follow-on action, even if user says "commit and push" — always human-initiated or a separate skill

WORKFLOW: git diff --staged → if motivation unclear, ask once: "Why were these changes made?" (skip on "skip"/"just commit"/no reason) → draft message → present full message for approval (skip only if user says so) → commit via heredoc

APPROVAL IS PER-MESSAGE, NOT PER-TURN: feedback on a draft ("cut that line", "wrong why
") is a revision request, never approval — even implicitly, even if the user sounds imp
atient. After every revision, re-present the new full message and wait for explicit aff
irmation of that exact text before running the commit. Never chain "revise → commit" in
 the same turn. If the user's own message already contains the exact final text and say
s to commit it verbatim, that counts as approval — otherwise assume no.

Body = why only. One paragraph, 2-4 sentences. A second paragraph needs its own justification, not "more context."

The why must be the actual motivation — a plan, constraint, or problem — not the technical change restated as if it were the reason. If the draft "why" just re-describes what the diff does, that's not a why yet; dig for the real motivation before presenting it.

FORMAT:
<type>[(scope)]: <description>   (≤50 chars, imperative, no trailing period)
<type>!: <description>           (breaking change variant)

<why this changed — motivation, constraint, or bug — most critical section>

[notes: ONLY items that will still matter to someone reading git log/git blame months from now with no memory of this review — a breaking behavior, a follow-up with an owner, a caveat baked into the change itself. Test: if it's only useful to whoever is looking at the diff right now (e.g. "these related things were deliberately left out of scope"), that's PR-review material, not commit material — cut it. Ban: restating the diff in prose, "how it's tested," "what's not done yet," secondary fixes bundled in. If it's not actionable, cut it. Default: omit.]
[BREAKING CHANGE: <desc>]
[Co-authored-by: Name <email>]   (human co-authors only; never add for Claude)
[See: <url> or Description <<url>>]

Blank line after subject; wrap body at 72 chars; blank line before trailers.

TYPES: feat=new feature | fix=bug | refactor=restructure(no fix/feat) | perf=performance | build=build/deps | chore=maintenance | ci=CI config | docs=docs only | style=formatting(no logic change) | test=add/fix tests | revert=revert prior commit

EXAMPLE:
feat(auth): add token refresh on 401

Session expiry was silently logging users out mid-workflow with no
recovery path. Refreshing on 401 keeps sessions alive without forcing
re-authentication.

See: https://github.com/org/repo/issues/42
