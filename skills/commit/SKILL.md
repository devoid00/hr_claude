---
name: commit
description: >-
  Review the current git diff and create well-formed Conventional Commits
  (feat, fix, chore, refactor, docs, ...), grouping related changes into
  separate logical commits per component. Use when the user wants to commit
  staged/unstaged work ("commit this", "make a commit", "commit these
  changes"). Optional `prune` argument instead reviews all commits on the
  current branch since it diverged from main and consolidates many small
  intermediate commits (fix, wip, etc.) into a handful of clean Conventional
  Commits — use for "prune my commits", "clean up this branch's history",
  "squash these into feature commits" before opening a PR.
argument-hint: [prune]
allowed-tools: Bash, Read, AskUserQuestion
---

# commit — Conventional Commit authoring and branch history cleanup

Two modes:

| The user wants to... | Mode | Read |
|---|---|---|
| commit the current working-tree changes (staged/unstaged) as one or more grouped Conventional Commits | **commit** (default) | `references/commit.md` |
| consolidate/clean up/prune the current branch's existing commits into a small set of feature-level Conventional Commits | **prune** | `references/prune.md` |

Bare `/commit` (no argument) is **commit** mode. `/commit prune` is **prune** mode.

---

## Shared: Conventional Commits format

Every commit this skill creates follows:

```
<type>(<optional scope>): <imperative, lowercase summary, no trailing period>

<optional body: why, not what>
```

Types:
- `feat` — new capability visible to a user/consumer
- `fix` — bug fix
- `refactor` — code change that doesn't change behavior
- `chore` — maintenance (deps, tooling, config, cleanup) with no source behavior change
- `docs` / `test` / `style` / `perf` / `build` / `ci` — as named

Scope is optional, lowercase, names the component/area touched (e.g.
`feat(ticket): ...`). Summary is imperative mood ("add", not "added"/"adds"),
no more than ~72 chars, no trailing period.

Every commit created by this skill ends with the trailer used across this
repo's git workflow:
```
Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
```

## Shared: grouping principle

Group changes by **logical component/concern**, not by file-modified-time or
alphabetically. A component is usually: one skill/feature directory, one
concern (e.g. "docs updates" vs "new skill logic"), or one bug fix. Don't
split one coherent change into one-commit-per-file; do split when the diff
clearly contains unrelated concerns (e.g. a new skill + an unrelated README
fix).

## Shared: never skip confirmation

Both modes must show the user the proposed commit plan (groups, files, and
draft messages — or in prune mode, the new commit sequence) and get explicit
go-ahead **before** running any `git add` / `git commit` / `git reset`. Commit
grouping is a subjective judgment call, and prune mode rewrites history
outright — both deserve a check-in even though this skill's whole purpose is
committing.

Follow the Git Safety Protocol throughout: no `--no-verify`, no force-push,
never commit files that look like secrets (`.env`, credentials), always
create NEW commits rather than amending unless the user asks otherwise, and
never stage with `git add -A`/`git add .` — add specific files.
