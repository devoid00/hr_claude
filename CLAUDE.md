# hr_claude

Version-controlled Claude Code skills, packaged as the `hr` plugin. The repo is
also its own local marketplace (`.claude-plugin/`); installed with
`claude plugin install hr@hr`, it loads in place from this checkout, so edits apply
on `/reload-plugins`. Skills are invoked namespaced: `/hr:<skill>`.

## Conventions
- One skill per folder: `skills/<name>/SKILL.md` (+ optional `references/`, `templates/`, `scripts/`, `resources/`).
- Edit and commit skills **here**. `~/.claude/skills` is no longer linked to this repo — don't put plugin skills there.
- Refer to skills by namespaced name in skill text (`/hr:ticket`), not bare `/ticket`.
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
- **brainstorm** — pre-ticket idea work in the vault: explore an idea, research a question into a sourced note, capture quick ideas to an inbox, promote mature ideas to ticket/decide/wiki. Project notes → `projects/<p>/research/<topic>/`, others → `brainstorm/`. Router `skills/brainstorm/SKILL.md`; modes in `skills/brainstorm/references/{explore,research,capture,promote}.md`.
