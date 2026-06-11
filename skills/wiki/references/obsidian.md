# Wiki-Specific Obsidian Conventions

For general Obsidian syntax (wikilinks, callouts, frontmatter, embeds, tags), use the
`obsidian:obsidian-markdown` skill. This file covers only conventions specific to the
claude-wiki knowledge base.

## Wikilink targeting in this wiki

Obsidian resolves wikilinks by **filename** (without extension), case-insensitively.
It does NOT use the `title:` frontmatter property for resolution.

Use the kebab-case slug matching the filename — the same pattern the existing articles use:

```markdown
[[cap-theorem]]          ← resolves to cap-theorem.md ✓
[[extract-method]]       ← resolves to extract-method.md ✓
[[CAP Theorem]]          ← does NOT resolve to cap-theorem.md ✗ (space ≠ hyphen)
```

To display a human-readable label, use the pipe syntax:
```markdown
[[cap-theorem|CAP Theorem]]
```

Use `obsidian unresolved verbose` after editing to confirm no new broken links.

## Where wikilinks go

- **Related section**: every article ends with a Related section using `[[wikilinks]]`
- **First mention inline**: when a concept is first mentioned in an article, link it

Every wikilink is a graph edge in Obsidian. Dense, accurate wikilinks make the graph useful.

## Bases files

`.base` files are Obsidian database configuration — do not modify them.
Use `obsidian:obsidian-bases` if you need to create new Bases views for the wiki.

---

## Obsidian MCP server setup

MCP gives the `query` operation semantic/BM25 search via `mcp__obsidian__search_vault`.
Register one of these at user scope with `claude mcp add --scope user` (stored in
`~/.claude.json`), or add it to a project `.mcp.json`. MCP servers do **not** go in
`~/.claude/settings.json`.

### Option A: obsidian-mcp (recommended — no plugin, Obsidian doesn't need to run)

```sh
claude mcp add --scope user obsidian -- npx -y obsidian-mcp /Users/ed/Documents/notes
```

Equivalent `.mcp.json` entry:

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "obsidian-mcp", "/Users/ed/Documents/notes"]
    }
  }
}
```

Tools available: `search_vault`, `list_files_in_vault`, `get_file_contents`,
`patch_vault_file`, `append_to_vault_file`, `delete_vault_file`

### Option B: mcp-obsidian (requires Local REST API plugin + Obsidian running)

```sh
claude mcp add --scope user obsidian -e OBSIDIAN_API_KEY=<key-from-plugin> -- npx -y mcp-obsidian
```

Note: with Option A configured, prefer `obsidian-mcp` for search rather than the CLI,
since it works without Obsidian open.
