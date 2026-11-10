# hr_claude

Version-controlled Claude Code customizations (skills; later commands/hooks).

This repo is a Claude Code **plugin** (`hr`) and its own local **marketplace**
(`.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json`). Skills in
`skills/` are auto-discovered and invoked namespaced: `/hr:ticket`, `/hr:brainstorm`, …

The plugin loads **in place** from this checkout: edits apply on `/reload-plugins`
or the next session, no version bump or reinstall needed.

Install on a new machine:

```sh
claude plugin marketplace add ~/repos/hr_claude
claude plugin install hr@hr
```

For a one-off session against a different checkout: `claude --plugin-dir <path>`.

Obsidian note templates are still symlinked into the vault (Templater reads them there):

```sh
ln -s ~/repos/hr_claude/skills/travel/templates ~/Documents/notes/templates/travel
ln -s ~/repos/hr_claude/skills/ticket/templates ~/Documents/notes/templates/ticket
ln -s ~/repos/hr_claude/skills/wiki/templates   ~/Documents/notes/templates/wiki
ln -s ~/repos/hr_claude/skills/brainstorm/templates ~/Documents/notes/templates/brainstorm
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

### `brainstorm`
The step before tickets: exploring and researching ideas into vault notes.
Four auto-routed modes:

- **explore** — short interview, then a brainstorm note with angles, risks, and open questions.
- **research** — answer a focused question with web/API/vault sources into a cited research note.
- **capture** — append a quick idea to an inbox (`brainstorm/Inbox.md` or a project's `research/Inbox.md`).
- **promote** — hand a mature note to `ticket`, `decide`, or `wiki`, with links both ways.

Project notes go in `projects/<project>/research/<topic>/`; the rest in `brainstorm/`.
`brainstorm/Brainstorm.base` (copied from `skills/brainstorm/resources/` on first use)
lists them all. Router: `skills/brainstorm/SKILL.md`; modes: `skills/brainstorm/references/`.
