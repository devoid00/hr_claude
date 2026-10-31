---
name: ticket
description: >-
  Create, review, or validate project tickets in the Obsidian vault at
  ~/Documents/notes (Credo and other projects). Use when the user wants to
  write / draft / create a ticket, review / critique / check a ticket, or
  audit / sync / validate tickets against design docs (roadmap, use cases,
  requirements, decisions) — for example after a design document changes, or to find stale
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
ticket. After the user edits a design doc (roadmap/use cases/decisions), offer to
**sync**. Only run the next mode if they say yes.

---

## Shared context (used by all modes)

### Vault layout
- Vault root: `~/Documents/notes`
- Tickets live in: `projects/<project>/tickets/` (e.g. `projects/credo/tickets/`)
- Person pages: `individual/t/T.md`, `individual/ed/Ed.md`, `individual/nat/Nat.md`

Resolve all paths against the vault root so the skill works from any cwd. Prefer
the Obsidian-aware tools/skills when reading or writing vault notes.

### Projects
| Project  | `project:` value | tickets dir        | ID prefix | docs root            |
| -------- | ---------------- | ------------------ | --------- | -------------------- |
| Credo    | `Credo`          | `projects/credo/tickets/`    | `C`       | `projects/credo/`    |
| Cookbook | `Cookbook`       | `projects/cookbook/tickets/` | `CB`      | `projects/cookbook/` |

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
serves:                 # the use case(s) or requirement this moves forward
  - "[[UC15 - Compare where entities stand]]"   # or "[[Requirements#Launch acceptance (Phase 6)]]"
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
Why this ticket exists. Link what it serves (use case / requirement) and the relevant [[concepts/...]].

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
- `projects/credo/product/use-cases/*.md` — one note per use case (`UCnn`) or epic (`En`): frontmatter `phase`, `status` (built/partial/not-started), `scope` (launch/next/north-star), plus `## Acceptance criteria`. Dashboard: `product/Use Cases.base`
- `projects/credo/product/Requirements.md` — FR/NFR tables (traced to use cases) + `## Launch acceptance (Phase 6)`
- `projects/credo/product/Personas.md` — who the actors are
- *(The old `features/` notes were folded into the use cases on 2026-10-31; archived in `_archive/features/` — never link them.)*
- `projects/credo/Decisions.md` — decision tables (Decided / Undecided)
- `projects/credo/STATUS.md` — living session handoff + milestone checklist
- `projects/credo/Agent Workflow.md` — the agent roster + the prompting rules a good ticket follows
- `projects/credo/concepts/*.md` — design detail to link from tickets

### Quality bar (from Agent Workflow.md — applies to new + review)
A good ticket: is specific about file paths; is scoped to **one** layer/agent;
states what **not** to do; is completable in one session; has testable success
criteria; and is consistent with the **decided** decisions in `Decisions.md`.
