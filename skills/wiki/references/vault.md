# Vault Structure

Vault root: `/Users/ed/Documents/notes/`

## Wiki directory: `claude-wiki/`

| Path | Purpose |
|------|---------|
| `claude-wiki/CLAUDE.md` | Schema and governance rules for the wiki (source of truth) |
| `claude-wiki/_index.md` | Master index — one line per article, alphabetical |
| `claude-wiki/raw/` | Immutable source documents — never modify after first write |
| `claude-wiki/wiki/concepts/` | Single-concept articles: one technology, pattern, or term |
| `claude-wiki/wiki/topics/` | Broader topic guides, surveys, and how-tos |
| `claude-wiki/output/` | Generated reports, comparisons, slides |

## Rest of vault (for context)

| Path | Purpose |
|------|---------|
| `travel/` | Trip and destination notes |
| `templates/` | Obsidian templates (Templater plugin) |
| `projects/` | Active project pages |
| `daily-notes/` | Daily notes |
| `education/` | Learning and study notes |
| `individual/` | Personal notes |
| `lists/` | Reference lists |
| `recipes/` | Recipe notes |

## Naming conventions

- **File names**: `kebab-case.md` — no spaces, all lowercase
- **Concepts**: short noun phrases — `cap-theorem.md`, `extract-method.md`
- **Topics**: descriptive phrases — `system-design-url-shortener.md`, `psql-and-postgresql-basics.md`
- **Raw sources**: date-prefixed — `2026-06-03-source-name.md`
- **No nesting beyond** `wiki/concepts/` and `wiki/topics/` — keep the structure flat

## The immutability rule

Files in `raw/` are never edited after creation. If a source needs annotation, create a
new file alongside it (e.g. `2026-06-03-source-notes.md`) rather than editing the original.
The wiki articles are the layer where synthesis and annotation live.
