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

Body = why only. One paragraph, 2-4 sentences. A second paragraph needs its own justification, not "more context."

FORMAT:
<type>[(scope)]: <description>   (≤50 chars, imperative, no trailing period)
<type>!: <description>           (breaking change variant)

<why this changed — motivation, constraint, or bug — most critical section>

[notes: ONLY items forcing a reviewer decision — a caveat that changes review, a follow-up with an owner, a breaking behavior. Ban: restating the diff in prose, "how it's tested," "what's not done yet," secondary fixes bundled in. If it's not actionable, cut it. Default: omit.]
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
