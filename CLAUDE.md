# hr_claude

Version-controlled Claude Code skills. `skills/` is symlinked into the personal
skills dir (`~/.claude/skills → ~/repos/hr_claude/skills`), so skills here are
git-tracked and auto-loaded every session.

## Conventions
- One skill per folder: `skills/<name>/SKILL.md` (+ optional `references/`, `templates/`, `scripts/`, `resources/`).
- Edit and commit skills **here** — never create files directly in `~/.claude/skills` (it's a symlink).
- `skills/<name>/templates/` holds canonical Obsidian note templates, symlinked into the
  vault as `~/Documents/notes/templates/<name>/` (Templater reads them there; skills fill
  them directly). Edit templates here, not in the vault.
  Exception: `templates/decide/` in the vault is a **real folder** — the decide skill
  generates per-decision candidate templates there (vault data, not repo code); only
  decide's generic hub template lives in the repo (no symlink needed).
- Prefer one skill with auto-routed modes over several narrow skills when the tasks are related.

## Skills
- **ticket** — author / review / sync project tickets in the Obsidian vault (`~/Documents/notes`). Router `skills/ticket/SKILL.md`; modes in `skills/ticket/references/{new,review,sync}.md`. See README.md.
- **decide** — scaffold and run candidate-comparison decisions (housing search, job search, purchases) in `~/Documents/notes/decisions/`: criteria interview → per-decision schema + Candidates.base; add/update/compare/decide ops. Router `skills/decide/SKILL.md`.
- **commit** — review the working diff and create grouped Conventional Commits; optional `prune` mode consolidates a branch's commit history into feature-level commits. Router `skills/commit/SKILL.md`; modes in `skills/commit/references/{commit,prune}.md`.
