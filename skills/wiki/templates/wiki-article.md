---
title: <% tp.system.prompt("Article title (Title Case)") %>
tags: [<% tp.system.prompt("Tags (comma-separated, lowercase kebab-case, include the domain tag)") %>]
sources: [raw/<% tp.system.prompt("Primary raw source filename (e.g. 2026-06-03-source.md)") %>]
last_updated: <% tp.date.now("YYYY-MM-DD") %>
---

## Summary
<!-- 2–3 sentence overview. Must stand alone — a reader should understand the article without reading further. -->

## Key Concepts
<!-- Main technical content. Headers, tables, and code blocks freely; sub-sections for complex topics. -->

## Practical Notes
- <!-- operational gotchas; bullet points over prose -->

## Related
- [[article-slug|Article Title]] — one-line description of the relationship

## Sources
- [raw/source-file.md](../../raw/source-file.md) — brief description of what this source is
