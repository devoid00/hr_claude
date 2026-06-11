---
name: ticket
description: >-
  Create, review, or validate project tickets in the Obsidian vault at
  ~/Documents/notes (Credo and other projects). Use when the user wants to
  write / draft / create a ticket, review / critique / check a ticket, or
  audit / sync / validate tickets against design docs (roadmap, features,
  decisions) — for example after a design document changes, or to find stale
  or missing tickets and see "what's next".
argument-hint: new <description> | review <ticket-id> | sync [--report-only|--dry-run]
---

# ticket — author, review, and sync project tickets

This skill manages **tickets** for projects tracked in the Obsidian vault at
`~/Documents/notes`. A ticket is an actionable unit of work with frontmatter
(assignee, phase, status, …) and testable success criteria. It is also an
**agent task spec**: a good ticket is what you'd hand a Claude Code agent.

It has three **modes**. Detect the mode from the request, then read the matching
file in `references/` and follow it exactly:

| The user wants to…                                                              | Mode     | Read                  |
| ------------------------------------------------------------------------------- | -------- | --------------------- |
| write / draft / create / add a ticket; "new ticket for X"                       | **new**  | `references/new.md`    |
| review / critique / check / grade an existing ticket; "is C00X any good?"        | **review** | `references/review.md` |
| sync / validate / audit tickets; "a design doc changed"; "what's stale/missing" | **sync** | `references/sync.md`   |

**Disambiguation.** If the request is bare (`/ticket` with no verb) or genuinely
ambiguous, use the **AskUserQuestion** tool to offer the three modes as options —
**new** (draft a ticket), **review** (check one ticket), **sync** (validate all
tickets vs design docs) — before doing anything. Don't guess between *review* (one
ticket) and *sync* (all tickets) — they are different.

**Chaining (offer, don't auto-run).** After **new**, offer to **review** the new
ticket. After the user edits a design doc (roadmap/features/decisions), offer to
**sync**. Only run the next mode if they say yes.

---

## Shared context (used by all modes)

### Vault layout
- Vault root: `~/Documents/notes`
- Tickets live in: `tickets/<project>/` (e.g. `tickets/credo/`)
- Person pages: `individual/t/T.md`, `individual/ed/Ed.md`, `individual/nat/Nat.md`

Resolve all paths against the vault root so the skill works from any cwd. Prefer
the Obsidian-aware tools/skills when reading or writing vault notes.

### Projects
| Project  | `project:` value | tickets dir        | ID prefix | docs root            |
| -------- | ---------------- | ------------------ | --------- | -------------------- |
| Credo    | `Credo`          | `tickets/credo/`   | `C`       | `projects/credo/`    |
| Cookbook | `Cookbook`       | `tickets/cookbook/`| `CB`      | (n/a)                |

`project` **defaults to Credo** when unspecified.

### Credo team
The two Credo developers are **T** (`[[T]]`) and **Ed** (`[[Ed]]`). `assignee`
must be one of these links or blank. (Nat has a person page but is not a Credo dev.)

### Ticket frontmatter schema
The canonical skeleton is `templates/Ticket Template.md` (vault:
`templates/ticket/Ticket Template.md`); this table documents the allowed values.
```yaml
type: ticket            # always — this is the Base's filter
id: C006                # <prefix><zero-padded number>, matches the filename prefix
project: Credo
phase: 2                # integer phase number
status: todo            # backlog | todo | in-progress | blocked | review | done
priority: high          # high | medium | low
assignee: "[[Ed]]"      # [[T]] | [[Ed]] | (blank)
feature: "[[projects/credo/features/Phase 2 - Core Views]]"
depends_on:             # optional — list of [[ticket]] links this blocks on
  - "[[credo/C003 - Housing Map API Endpoint]]"
agent: backend          # optional — db | backend | frontend | test | devops | geospatial
created: 2026-06-07
updated: 2026-06-07
```

Body sections, in order:
```markdown
# C006 - <Title>

## Background
Why this ticket exists. Link the feature and the relevant [[concepts/...]].

## Implementation
Concrete, file-path-specific steps. One layer only. End with an explicit
"Do not …" boundary. Keep it completable in one session (≲10 files).

## Success Criteria
- [ ] #task [[Credo]] [[Ed]] C006: a testable, objective criterion
- [ ] #task [[Credo]] [[Ed]] C006: another one

## Concepts
- [[concepts/...]]
```

### Two status concepts (don't conflate them)
- **Ticket status** = frontmatter `status`. Drives the `Tickets.base` board.
- **Task-checkbox status** = the `- [ ] / - [/] / - [x]` state of each `#task`
  line. Drives the Tasks-plugin queries on the T/Ed person pages. Including
  `[[Assignee]]` in a `#task` line is what makes it appear on that person's page.

### Status vocabulary (Jira-like)
`backlog` → `todo` → `in-progress` → `blocked` → `review` → `done`

### Design docs (Credo) — the source of truth modes read against
- `projects/credo/Roadmap.md` — phase table (phase → status: Done/Next/…)
- `projects/credo/features/Features.md` — phase → feature → status table
- `projects/credo/features/Phase N - *.md` — per-feature acceptance criteria + `## Linked Tickets`
- `projects/credo/Decisions.md` — decision tables (Decided / Undecided)
- `projects/credo/STATUS.md` — living session handoff + milestone checklist
- `projects/credo/Agent Workflow.md` — the agent roster + the prompting rules a good ticket follows
- `projects/credo/concepts/*.md` — design detail to link from tickets

### Quality bar (from Agent Workflow.md — applies to new + review)
A good ticket: is specific about file paths; is scoped to **one** layer/agent;
states what **not** to do; is completable in one session; has testable success
criteria; and is consistent with the **decided** decisions in `Decisions.md`.
