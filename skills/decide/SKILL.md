---
name: decide
description: >-
  Run structured decisions in the Obsidian vault (~/Documents/notes/decisions/):
  one note per candidate with criteria as frontmatter, a Candidates.base
  aggregating them, a hub note holding the verdict. Operations: new — interview
  for criteria, then scaffold a decision workspace ("help me decide", "I'm
  looking for a new place to live", "start a job search", "compare laptops");
  add a candidate by name or listing URL ("add a candidate", "add this
  apartment", "found a job posting"); update a candidate after a visit or
  research; compare for a ranked verdict ("compare the candidates", "which
  should I pick"); decide to record the final choice.
argument-hint: new <decision> [domain] | add <name|url> [to <decision>] | update <candidate> | compare [<decision>] | decide <candidate>
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Skill, AskUserQuestion
---

# decide — structured candidate comparison

Scaffolds and runs decision workspaces in the vault: a folder per decision, a note per
candidate, criteria in frontmatter, an Obsidian Base for aggregation, a hub note for
the running verdict. Generalizes the travel pattern (candidates + Bases) to housing
searches, job searches, purchases, or anything with comparable options.

Detect the operation, then read the matching reference and follow it:

| Operation | Read |
|-----------|------|
| `new` — define criteria + scaffold a decision | `references/scaffold.md` (uses `references/domains.md`) |
| `add` / `update` — manage candidates | `references/candidates.md` |
| `compare` / `decide` — verdicts | `references/compare.md` |

Active decisions (hubs with `type: decision`):
!`grep -rl --include="*.md" "^type: decision$" /Users/ed/Documents/notes/decisions 2>/dev/null | sort`

If the target decision is ambiguous (multiple active, none named), ask.

---

## Shared context

### Vault layout

```
decisions/
├── Decisions.base                   # overview of all decisions (created on first `new`)
└── <Decision Name>/                 # e.g. Housing 2026/
    ├── <Decision Name>.md           # hub: criteria + weights, Base embeds, ## Verdict
    ├── Candidates.base
    └── candidates/
        └── <Candidate>.md

templates/decide/                    # real vault folder (NOT a repo symlink)
└── <decision-slug>-candidate.md     # generated per-decision candidate template
```

Vault root: `/Users/ed/Documents/notes`. Always use full vault paths in Base embeds.

### Frontmatter

**Hub** (`type: decision`): `domain`, `status: deciding | decided | abandoned`,
`created`, `chosen:` (wikilink to the winning candidate, set by `decide`).

**Candidate** (`type: decision-candidate`): `decision: "[[<Decision Name>]]"` (drives
the Base's `file.hasLink` filter — the hub's alias must match exactly), `status`,
`url`, `added`, plus the decision's criteria keys. Rate-able criteria use
`rating_<name>` (integer 1–5) so the score formula can target them.

**`status` vocabulary is per-decision** — defined by the domain pack (see
`references/domains.md`), editable during the interview, and recorded in the hub's
Criteria section. The Base's Board view groups by it, so e.g. a job search doubles as
an application tracker.

### Creating notes

The candidate schema is per-decision, so its template is **generated into the vault**
at `templates/decide/<slug>-candidate.md` (Templater auto-applies it when Ed creates
an empty note in that decision's `candidates/` folder by hand). The hub template is
generic and lives in this skill at `templates/decision-hub.md`.

Skill-created notes: read the template, replace each `<% tp.* %>` placeholder with the
concrete value, Write the file. Never run `obsidian templater:create-from-template`
from the skill — it pops interactive dialogs in the Obsidian UI (manual path only).

Prefer the Obsidian-aware plugin skills when relevant: `obsidian:obsidian-bases` for
`.base` syntax, `obsidian:obsidian-markdown` for note syntax, `obsidian:defuddle` for
fetching listing URLs.

## Reference files

| File | Contents |
|------|----------|
| `references/scaffold.md` | `new`: criteria interview → folder/hub/template/Base generation, Templater mapping |
| `references/candidates.md` | `add` (URL best-effort + manual) and `update` |
| `references/compare.md` | `compare` (ranked verdict → hub) and `decide` (record choice) |
| `references/domains.md` | Starter criteria packs + status vocabularies per domain |
