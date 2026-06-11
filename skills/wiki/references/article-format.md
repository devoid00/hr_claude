# Article Format

The canonical frontmatter + section skeleton is `templates/wiki-article.md` (symlinked
into the vault as `templates/wiki/wiki-article.md`). To create an article, read the
template, replace the `<% tp.* %>` placeholders and HTML comments with real content,
and Write the file. Templater auto-applies this template when an empty note is created
by hand under `claude-wiki/wiki/`.

## Frontmatter (required on every wiki article)

```yaml
---
title: Human-Readable Title
tags: [domain, subtopic, tool-name]
sources: [raw/source-file.md]
last_updated: YYYY-MM-DD
---
```

Field rules:
- `title`: Title Case; the display name used in `_index.md` entries and wikilink pipe
  labels (wikilinks themselves target the filename slug — see `references/obsidian.md`)
- `tags`: lowercase kebab-case; always include the domain tag (e.g. `llm`, `refactoring`, `system-design`, `obsidian`)
- `sources`: list of `raw/` paths backing this article; at least one required; add more as new sources are ingested
- `last_updated`: update this date whenever article content changes

## Article sections (in order)

```markdown
## Summary
2–3 sentence overview. Must stand alone — a reader should understand the article
without reading further.

## Key Concepts
Main technical content. Use headers, tables, and code blocks freely.
Break into sub-sections for complex topics.

## Practical Notes
- Operational gotchas and things that trip people up
- Prefer bullet points over prose here
- Reference the source file if a specific note comes from one source

## Related
[[article-slug|Article Title]] — one-line description of the relationship
[[other-slug|Another Related Concept]] — how they connect

## Sources
- [raw/source-file.md](../../raw/source-file.md) — brief description of what this source is
```

## When to use concepts/ vs topics/

| `concepts/` | `topics/` |
|-------------|-----------|
| Single well-scoped term or pattern | Broader guide, survey, or workflow |
| `cap-theorem.md` | `system-design-web-crawler.md` |
| `extract-method.md` | `refactoring-catalog.md` |
| `bjt-transistors.md` | `psql-and-postgresql-basics.md` |

When in doubt: if you could give it as a 5-minute lightning talk, it's a concept. If it
needs 20 minutes with a demo, it's a topic.

## _index.md entry format

One line per article, alphabetical by title:
```
- [Article Title](wiki/concepts/article-title.md) — one-line summary under 120 chars
```

Alphabetization rules:
- Sort by display title, ignoring "The", "A", "An" at the start
- Use the file's `title:` frontmatter as the display name
- Paths are relative to `claude-wiki/` root

## Updating existing articles

When new source material touches an existing article:
1. Add new info under the appropriate section — do not replace, extend
2. Add the new raw/ file to `sources:` list
3. Update `last_updated` to today
4. If a Practical Note becomes outdated, mark it with `> [!warning] Outdated as of YYYY-MM-DD` before removing
