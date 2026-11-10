---
name: brainstorm
description: >-
  Early-stage idea work in the Obsidian vault at ~/Documents/notes — the step
  before tickets. Use when the user wants to brainstorm or explore an idea
  ("let's brainstorm", "explore this idea", "think through X"), research a
  question or topic and write it up ("research X", "look into", "what exists
  for", "prior art on"), quickly jot an idea down ("capture this", "note this
  idea", "add to my ideas"), or turn a mature brainstorm into tickets, a
  decision, or wiki articles ("promote this", "turn this into tickets").
argument-hint: explore <idea> | research <question> | capture <idea> | promote <note>
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Skill, AskUserQuestion, Agent
---

# brainstorm — explore, research, capture, promote ideas

This skill covers the **pre-ticket** stage: an idea is still fuzzy, open
questions are welcome, and opinions are fine as long as they're marked as such.
Its output is notes in the vault. When an idea is ready to act on, it hands off
to another skill:

| Skill     | Holds                                       | Relation                                  |
| --------- | ------------------------------------------- | ----------------------------------------- |
| brainstorm | exploratory notes, tied to an idea/project | this skill                                |
| ticket    | scoped, actionable work                     | `promote` → ticket **new**                |
| decide    | candidate comparisons                       | `promote` → decide **new**                |
| wiki      | durable general knowledge (`claude-wiki/`)  | `promote` → wiki **ingest**; never write there directly |

Detect the mode from the request, then read the matching file in `references/`
and follow it:

| The user wants to…                                                       | Mode         | Read                     |
| ------------------------------------------------------------------------ | ------------ | ------------------------ |
| brainstorm / explore / think through an idea; "what if we…"              | **explore**  | `references/explore.md`  |
| research / look into / find prior art on a specific question or topic     | **research** | `references/research.md` |
| jot down / capture / note an idea quickly, no research                   | **capture**  | `references/capture.md`  |
| promote / turn into tickets / start a decision / save sources to the wiki | **promote**  | `references/promote.md`  |

**Disambiguation.** If the request is bare (`/hr:brainstorm` with nothing else) or
ambiguous, use **AskUserQuestion** to offer the four modes. A short idea with no
verb ("brainstorm: QR codes for storage bins") is **explore**; a question with a
factual answer ("what APIs does Regulations.gov have?") is **research**.

**Chaining (offer, don't auto-run).** capture → offer explore. explore → offer
research on the open questions. research → offer promote when the open questions
are mostly answered. Run the next mode only if the user says yes.

---

## Shared context

### Vault
- Root: `~/Documents/notes`. Resolve all paths against it so the skill works from any cwd.
- Use `obsidian:obsidian-markdown` for syntax (wikilinks, callouts, frontmatter),
  `obsidian:defuddle` to read web pages, and `obsidian:obsidian-cli` for vault search
  when Obsidian is running (fall back to Grep/Glob when it isn't).

### Where notes go
- **Project idea** → `projects/<project>/research/<topic-folder>/<Title>.md`.
  This matches existing Credo notes, e.g.
  `projects/credo/research/existing-projects/Regulations.md`. Reuse an existing
  topic folder when one fits (list `research/` first); otherwise create a short
  kebab-case folder (`denver-politics`, `existing-projects`).
- **No project** → `brainstorm/<Title>.md` (top-level vault folder).
- Projects are the folders under `projects/` (Credo → `projects/credo/`, Cookbook →
  `projects/cookbook/`, `claude`, `financial-planning`, …). The `project:` value is
  the Title Case name (`Credo`). If it's unclear whether an idea belongs to a
  project, ask — don't default to Credo here.
- **Inboxes** (capture mode): `brainstorm/Inbox.md`, or
  `projects/<project>/research/Inbox.md`.

### Filenames
Human-readable Title Case, spaces allowed: `Regulations.md`, `QR Bin Tracker.md`.
Not kebab-case (that's the wiki's convention). Before creating, Grep the vault for
the title and key terms — if a related note exists, offer to extend it instead.

### Frontmatter schema
Canonical skeletons: `templates/Brainstorm Template.md` and
`templates/Research Template.md` (vault: `templates/brainstorm/`). When writing a
note directly, fill the values yourself — don't leave Templater `<% %>` tags in.
```yaml
type: brainstorm        # brainstorm (explore) | research (research)
project: Credo          # Title Case project name, or blank
status: exploring       # seed | exploring | researched | promoted | parked
tags: []
sources: []             # URLs and [[vault links]] actually used
related: []             # [[links]] to related brainstorm/research/design notes
promoted_to: []         # [[ticket]] / [[decision hub]] / [[wiki article]] made from this
created: 2026-11-09
updated: 2026-11-09
```
Status meanings: **seed** = just an idea · **exploring** = brainstorm written, open
questions remain · **researched** = facts gathered · **promoted** = handed off to
ticket/decide/wiki · **parked** = deliberately shelved (add a one-line reason in the body).
Always bump `updated:` when you edit a note.

### Dashboard
`brainstorm/Brainstorm.base` lists every `type: brainstorm` / `type: research`
note across the vault (project notes included). If it doesn't exist, copy
`resources/Brainstorm.base` from this skill to that path the first time you write
a note. Use `obsidian:obsidian-bases` if you need to change it.

### Writing rules (all modes)
- Keep **facts** (with a source) apart from **Claude's opinions or guesses** — put
  the latter in a `> [!note] Take` or `> [!question]` callout.
- Link generously with `[[wikilinks]]` to existing vault notes (project design docs,
  `concepts/`, wiki articles). For wiki articles, link by kebab-case filename with a
  pipe label: `[[cap-theorem|CAP Theorem]]`.
- Open questions are `- [ ]` checkboxes so they can be ticked off as research answers them.
- After writing, tell the user the path and a two-line summary. Don't paste the whole note.
