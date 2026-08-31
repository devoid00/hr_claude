---
aliases:
  - <% tp.file.title %>
type: decision
domain: <% tp.system.prompt("Domain (e.g. housing, job-search, purchase)") %>
status: deciding
created: <% tp.date.now("YYYY-MM-DD") %>
chosen:
cssclasses:
  - wide-note
tags:
  - decision
  - <% tp.system.prompt("Domain tag (e.g. housing)") %>
---

> [!info] Decision at a glance
> **Status:** deciding  |  **Started:** <% tp.date.now("YYYY-MM-DD") %>  |  **Candidates:** see views below

## Criteria

| Criterion | Frontmatter key | Kind | Weight |
| --------- | --------------- | ---- | ------ |
<!-- one row per criterion; Kind = fact (number/text/date/coordinates) or rating (1–5); Weight blank if unscored -->

**Status flow:** <!-- e.g. candidate → shortlist → visited → chosen / rejected -->

## Candidates

![[decisions/<% tp.system.prompt("Folder name (e.g. Housing 2026)") %>/Candidates.base#Table]]

## Board

![[decisions/<% tp.system.prompt("Folder name (same)") %>/Candidates.base#Board]]

## Verdict

<!-- `compare` rewrites this section (dated). `decide` records the final choice. -->
