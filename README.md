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
