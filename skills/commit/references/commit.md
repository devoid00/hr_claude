# commit mode

Turns the current working-tree changes (staged and/or unstaged) into one or
more grouped Conventional Commits.

## 1. Gather the diff
- `git status` — see staged, unstaged, and untracked files.
- `git diff` and `git diff --staged` — full content of every change.
- `git log --oneline -10` — recent style precedent, informational only; this
  skill standardizes on Conventional Commits regardless of what's already there.
- If the tree is completely clean, say so and stop — nothing to commit.

## 2. Group into components
Read through the diff and cluster files/hunks into logical groups. Typical
groupings: one group per skill/feature directory touched, one group for
top-level docs (e.g. `CLAUDE.md`/`README.md`) if they're just describing
other groups' changes, one group per unrelated fix.

If a single file's changes clearly serve two unrelated concerns, prefer
keeping the file whole in the group it predominantly belongs to and flag the
imperfection to the user, rather than attempting a hunk-level split — that's
a judgment call for the user to make, not something to guess at silently.

## 3. Draft messages
For each group, draft one Conventional Commit message per the format in
`SKILL.md`. Include a body only when the "why" isn't obvious from the summary
plus diff — don't restate what the diff already shows.

## 4. Confirm with the user
Present the plan as a short numbered list: group name → files → draft
message. Wait for confirmation (or requested edits) before touching git.
Reach for AskUserQuestion only if there's a real ambiguity to resolve (e.g.
"should this file's changes split across two groups?"); otherwise a plain
text summary and waiting for a reply is enough since this is a single
yes/no gate.

## 5. Execute
For each group, in order:
- `git add <specific files>` — never `git add -A`/`git add .`.
- `git commit -m "$(cat <<'EOF' ... EOF)"` with the drafted message plus the
  `Co-Authored-By` trailer.
- `git status` after each commit to confirm the working tree matches
  expectations before moving to the next group.

## 6. Report
Show the resulting `git log --oneline -n <count>` for the commits just made.
