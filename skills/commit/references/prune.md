# prune mode

Rewrites the current branch's own commit history (since it diverged from its
base) into a small number of clean, Conventional-Commit-formatted commits.
This is a **history-rewriting operation** — treat it with the same caution as
`git reset --hard`/an interactive rebase: always confirm before executing,
always leave a recovery path, never force-push as part of this skill.

## 1. Preconditions
- `git status` — working tree must be clean (no staged/unstaged changes). If
  not, stop and ask the user to commit or stash first — do not stash on their
  behalf without asking.
- Determine the base: try `git merge-base HEAD main`, falling back to
  `origin/main`, then `master`/`origin/master`. If none resolve, ask the user
  what the branch was cut from.
- If `HEAD` is already at the base, or there's exactly one commit ahead of
  it, say so and stop — there's nothing to consolidate.

## 2. Show what will change
- `git log --oneline <base>..HEAD` — the existing commit sequence.
- `git diff <base>..HEAD` — the full accumulated change.
- Check whether the branch has an upstream:
  `git rev-parse --abbrev-ref --symbolic-full-name @{u}` (ignore errors). If
  it does, tell the user up front that landing this locally will require a
  force-push to update the remote afterward — and that this skill will not
  push or force-push for them; that's a separate, explicit step they'd need
  to request.

## 3. Group and draft
Apply the same grouping and Conventional Commit drafting process as
`references/commit.md` (§2-3), but treat `git diff <base>..HEAD` as the
working diff — group the *net* accumulated change into feature-level
commits, ignoring the intermediate fix-up history. The goal is a clean
record of what the branch adds, not how it got there along the way.

## 4. Confirm
Show: the original commit list (from step 2) alongside the proposed new
commit list (messages only — the user already saw the full diff in step 2).
Get explicit go-ahead. This is the hard confirmation gate for this mode — do
not proceed without it.

## 5. Record a recovery point, then execute
- Note the current `git rev-parse HEAD` and tell the user this is the
  original tip, recoverable via `git reset --hard <sha>` if anything looks
  wrong afterward.
- Create a backup ref as a purely additive safety net:
  `git branch <branch>-backup-<YYYYMMDD>`.
- `git reset --soft <base>` — uncommits everything on the branch while
  keeping the full accumulated diff staged.
- For each group, in dependency order (e.g. an underlying change before the
  thing that depends on it): `git add <files>`, then `git commit` with the
  drafted message plus the `Co-Authored-By` trailer.
- If the working tree isn't empty after committing all groups, something was
  missed — check `git status` and fold the remainder into the most relevant
  existing group rather than leaving stray uncommitted changes.

## 6. Report
Show `git log --oneline <base>..HEAD` (the new, clean sequence) and the
backup branch name. Remind the user that pushing — especially force-pushing
an already-published branch — is their call to make separately; this skill
does not do it for them.
