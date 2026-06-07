# hr_claude

Version-controlled Claude Code skills. `skills/` is symlinked into the personal
skills dir (`~/.claude/skills → ~/repos/hr_claude/skills`), so skills here are
git-tracked and auto-loaded every session.

## Conventions
- One skill per folder: `skills/<name>/SKILL.md` (+ optional `reference/`, `scripts/`, `resources/`).
- Edit and commit skills **here** — never create files directly in `~/.claude/skills` (it's a symlink).
- Prefer one skill with auto-routed modes over several narrow skills when the tasks are related.

## Skills
- **ticket** — author / review / sync project tickets in the Obsidian vault (`~/Documents/notes`). Router `skills/ticket/SKILL.md`; modes in `skills/ticket/reference/{new,review,sync}.md`. See README.md.
