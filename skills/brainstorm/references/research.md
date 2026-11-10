# Mode: research — answer a question, write a sourced note

Goal: one research note that answers a focused question, with every fact cited.
Model it on `projects/credo/research/existing-projects/Regulations.md`: concrete,
data-model-level detail, worked examples, and a clear "what this means for us".
Read `SKILL.md` first for paths, schema, and writing rules.

## Procedure

### 1. Frame the question
- Pin down the question in one sentence. If the user handed over a brainstorm
  note, the questions are its unchecked *Open questions*. Confirm which ones to
  cover if there are many.
- Work out the project (or none) and the topic folder (`SKILL.md` → Where notes go).

### 2. Check the vault first
- Grep `projects/<project>/`, `brainstorm/`, and `claude-wiki/_index.md` for the topic.
- Link what's relevant and don't re-research what's already well covered there —
  summarize and link instead.

### 3. Research
Pick the cheapest source of truth for each part of the question:
- **WebSearch** to find sources; send independent searches together in one turn.
- **`obsidian:defuddle`** to read pages (instead of WebFetch). WebFetch is fine for raw `.md` / JSON.
- **Primary sources beat summaries**: official docs, public APIs (call them with
  `curl` when that's the quickest way to see real data — like the Regulations.gov
  `DEMO_KEY` example), source repos, specs, papers.
- **Big multi-source topics** (market scans, "compare every X", literature reviews):
  offer the `anthropic-skills:deep-research` skill instead of doing it inline.

Record every URL you actually rely on — they go in `sources:` and in the Sources section.

### 4. Write the note
Fill `templates/Research Template.md`: `type: research`, `status: researched`
(or `exploring` if big questions are still open). Sections:
- **TL;DR** — 3–5 bullets answering the question.
- **Findings** — organized by sub-question. Tables for structured comparisons. Cite inline with links.
- **How it relates to <project>** — implications, fit, conflicts with `Decisions.md`.
  This is opinion, so put it in a `> [!note] Take` callout. Omit the section if there's no project.
- **Open questions** — checkboxes for what's still unknown.
- **Sources** — list with one-line notes on what each one gave you.

Mark anything unverified ("couldn't confirm", "as of <date>"). Create
`brainstorm/Brainstorm.base` if it's missing.

### 5. Link back
- If it came from a brainstorm note: add each note to the other's `related:`, tick
  the open questions it answered (append ` → [[<Research Note>]]`), bump both `updated:`.
- Offer **promote** when the idea looks ready, or more research on what's left.
- If a source is broadly useful beyond this idea, mention that promote → wiki can ingest it.
