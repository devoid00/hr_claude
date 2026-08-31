# hr_claude

Version-controlled Claude Code customizations (skills; later commands/hooks).

`skills/` is symlinked into Claude Code's personal skills dir, so everything here
is both **git-tracked** and **auto-discovered** every session:

```
~/.claude/skills  →  ~/repos/hr_claude/skills
```

Recreate the symlinks on a new machine:

```sh
ln -s ~/repos/hr_claude/skills ~/.claude/skills
# Obsidian note templates (Templater reads these from the vault):
ln -s ~/repos/hr_claude/skills/travel/templates ~/Documents/notes/templates/travel
ln -s ~/repos/hr_claude/skills/ticket/templates ~/Documents/notes/templates/ticket
ln -s ~/repos/hr_claude/skills/wiki/templates   ~/Documents/notes/templates/wiki
```

## Skills

### `ticket`
Author, review, and sync project tickets in the Obsidian vault at
`~/Documents/notes`. One skill, three auto-routed modes:

- **new** — create a well-formed ticket, grounded in the project's design docs.
- **review** — grade a ticket against the quality bar and suggest fixes.
- **sync** — reconcile tickets with design docs; auto-fix safe drift, flag the rest.

Router: `skills/ticket/SKILL.md`; modes: `skills/ticket/references/{new,review,sync}.md`.
See the vault's `tickets/Tickets Overview.md` for the ticket schema and the
`Tickets.base` dashboard.

### `decide`
Generic candidate-comparison workspaces in `~/Documents/notes/decisions/` — housing
search, job search, purchases. `new` interviews for criteria (starter packs per
domain) and scaffolds a folder + hub + per-decision candidate template + Candidates.base
(table/cards/board/map views, optional weighted score column); then `add` (URL
best-effort or manual), `update` (after viewings/interviews), `compare` (ranked
verdict into the hub), `decide` (record the choice).

### `commit`
Review the current git diff and create grouped Conventional Commits (feat,
fix, chore, refactor, ...) — one commit per logical component, always
confirmed with you before anything is staged or committed. Optional `prune`
argument instead reviews the current branch's commits since it diverged from
main and consolidates them into a small set of feature-level commits, for
cleaning up history before a PR (rewrites local history; leaves a backup ref;
never force-pushes on its own).

Router: `skills/commit/SKILL.md`; modes: `skills/commit/references/{commit,prune}.md`.
