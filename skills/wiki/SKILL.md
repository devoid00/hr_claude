---
name: wiki
description: >-
  Manage and query the Obsidian-based LLM wiki at ~/Documents/notes/claude-wiki/
  (Karpathy three-layer pattern: raw sources → wiki articles → _index.md).
  Operations: ingest a URL, file, or pasted text ("add to wiki", "ingest this",
  "save this to the wiki"); query wiki content with citations ("what does the
  wiki say about", "what do I know about"); lint for broken links, orphans,
  dead-ends, and stale articles ("check wiki health", "audit wiki"); prune to
  fix broken links and orphans ("prune", "clean up the wiki").
argument-hint: ingest <url | path | text> | query <question> | lint | prune [--dry-run]
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Skill, AskUserQuestion, mcp__obsidian
---

# Wiki Skill

Manages the LLM wiki at `/Users/ed/Documents/notes/claude-wiki/`. See reference files
for vault structure, article format, and Obsidian conventions.

Plugin skills to invoke when needed:
- `obsidian:defuddle` — fetch clean markdown from a URL (use instead of WebFetch)
- `obsidian:obsidian-cli` — vault operations via the Obsidian CLI
- `obsidian:obsidian-markdown` — correct Obsidian syntax for notes, wikilinks, callouts

Vault root: `/Users/ed/Documents/notes`. All `obsidian` CLI commands require Obsidian
to be running.

Wiki state:
!`echo "Articles: $(find /Users/ed/Documents/notes/claude-wiki/wiki -name '*.md' 2>/dev/null | wc -l | tr -d ' ') | Raw sources: $(ls /Users/ed/Documents/notes/claude-wiki/raw/*.md 2>/dev/null | wc -l | tr -d ' ') | Index entries: $(grep -c '^\- \[' /Users/ed/Documents/notes/claude-wiki/_index.md 2>/dev/null || echo 0)"`

---

## ingest `<url | path | text>`

Process new source material into the wiki.

**Step 1 — Acquire source content**

- **URL**: Use `obsidian:defuddle` to fetch clean markdown:
  ```bash
  defuddle parse <url> --md
  ```
  Write output to a new `raw/` file with a metadata header:
  ```
  ---
  source: <url>
  fetched: <YYYY-MM-DD>
  type: web
  status: raw
  ---
  <defuddle output>
  ```
  Filename: `raw/<date>-<slug>.md` (date-prefixed, kebab-case slug from URL)

- **Local path**: Read the file. Write a copy to `raw/<date>-<slug>.md` with header
  (`source: file://<path>`, `imported: <date>`, `type: local`).

- **Pasted text**: Write directly to `raw/<date>-<slug>.md` with header
  (`source: manual`, `created: <date>`, `type: text`).

**Step 2 — Analyze source**

Extract from the content:
- Core concepts and definitions worth a dedicated article
- Practical notes, gotchas, operational insights
- Named entities (tools, systems, papers, authors)
- Cross-link candidates — check `_index.md` for existing articles to link to

**Step 3 — Determine scope**

Read `_index.md`. For each extracted concept decide:
- Update an existing article (add content, update `sources:` and `last_updated`)
- New focused concept → `wiki/concepts/<slug>.md`
- Broader guide or survey → `wiki/topics/<slug>.md`

**Step 4 — Write/update articles**

Write article files directly using the Write or Edit tool (CLI `create` has content length
limits unsuitable for full articles). Follow `references/article-format.md` for structure.
Use `obsidian:obsidian-markdown` for correct wikilink, callout, and frontmatter syntax.

Rules:
- Every claim traces to a `raw/` file in `sources:` frontmatter
- Cross-references use `[[wikilinks]]` targeting the kebab-case filename slug, with a
  pipe label for readability: `[[cap-theorem|CAP Theorem]]` (Obsidian resolves by
  filename, not `title:` — see `references/obsidian.md`)
- Never edit files in `raw/` after creation

**Step 5 — Update `_index.md`**

Use the Edit tool to insert the new entry at the correct alphabetical position:
```
- [Title](wiki/.../slug.md) — one-line summary
```

---

## query `<question>`

Answer a question grounded in wiki content with citations.

**Step 1 — Search**

Search within the wiki folder for relevance:
```bash
obsidian search:context query="<keywords>" path="claude-wiki/wiki" limit=10
```
If the `obsidian` MCP server is configured, prefer `mcp__obsidian__search_vault`
instead — it works without Obsidian running (see `references/obsidian.md`).
Also scan `_index.md` directly for article titles matching the question.

**Step 2 — Read top matches**

Read the 3–5 most relevant articles in full.

**Step 3 — Synthesize answer**

- Ground every claim in a specific article, cite with `[[Article Title]]`
- Note explicitly what the wiki does NOT cover — gaps are useful signal
- If the question is outside wiki coverage, say so and offer to ingest sources to fill the gap

---

## lint

Full wiki health audit using the Obsidian CLI.

**Broken wikilinks** — links that resolve to no file in the vault:
```bash
obsidian unresolved verbose
```
Filter results to wiki/ sources if needed.

**Orphan articles** — wiki files with no incoming links from any other file:
```bash
obsidian orphans | grep "claude-wiki/wiki"
```

**Dead-end articles** — wiki files with no outgoing links (should be rare; most articles
should link to related concepts):
```bash
obsidian deadends | grep "claude-wiki/wiki"
```

**Manual quality pass:**
- Articles with `last_updated` > 90 days old → potentially stale
- Articles with only one `sources:` entry → low confidence
- Concepts referenced in prose across multiple articles but with no dedicated article → gap candidates

Report as four sections: **Broken links** | **Orphans** | **Dead-ends** | **Quality flags**

---

## prune `[--dry-run]`

Fix issues surfaced by lint.

**Step 1** — Run lint to get the full issue list.

**Step 2 — Fix unresolved wikilinks**

For each broken `[[link text]]`, search for an article covering the same concept:
```bash
obsidian search query="<link text>" path="claude-wiki/wiki" limit=5
```
- If a match is found: the link target doesn't match the article's filename slug —
  rewrite the link as `[[<filename-slug>|<readable label>]]` (Obsidian resolves by
  filename, not `title:`)
- If no match: remove the `[[...]]` brackets, leave the text as plain prose

**Step 3 — Fix orphan articles**

For each orphan in `claude-wiki/wiki/`:
- Has proper `title:` and `sources:` frontmatter → add to `_index.md` and add `[[wikilinks]]`
  from related articles
- Missing or malformed frontmatter → flag for manual review, do not auto-add

`--dry-run`: print all proposed changes without writing any files.

---

## Reference files

| File | Contents |
|------|----------|
| `references/vault.md` | Vault paths, folder purposes, naming conventions |
| `references/article-format.md` | YAML frontmatter schema, article sections, _index.md format |
| `references/obsidian.md` | Wiki-specific wikilink conventions and MCP server setup |
