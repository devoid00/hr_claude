# Mode: new — author a ticket

Goal: produce one well-formed ticket file grounded in the project's design docs,
following the quality bar in `SKILL.md`. Read `SKILL.md` first for the schema,
projects, team, and paths.

## Procedure

### 1. Gather inputs
- **project** — default `Credo`.
- **title / one-line description** — what the ticket is. If missing or vague, ask.
- Optional: **phase**, **assignee** (`[[T]]`/`[[Ed]]`), **priority**, **status**.

If the user gave a rich description, infer the rest and confirm at the end rather
than interrogating up front.

### 2. Allocate the next ID
List `tickets/<project>/`, take the max number among files matching the project's
ID prefix (Credo→`C`, e.g. `C001`…`C005`), add 1, zero-pad to 3 digits → e.g. `C006`.
The filename is `tickets/<project>/<ID> - <Title>.md`.

### 3. Ground the ticket in the design docs
Do this before writing — it's what makes the ticket accurate.
1. **Phase + feature.** Determine the phase (from the user, or infer from
   `Roadmap.md`/`Features.md`). Open `features/Features.md`, find the feature for
   that phase, and set `feature:` to that feature file's link. Open the feature
   file (`features/Phase N - *.md`) and read its **acceptance criteria** — your
   success criteria should map to the relevant ones.
2. **Decisions.** Open `Decisions.md`. **Honor decided decisions** (e.g. Leaflet
   not Mapbox; library-first; Git-repo source of truth). If an *undecided*
   decision blocks this work, say so in Background and consider `status: blocked`.
3. **Concepts.** Open the `concepts/*.md` the work touches; collect links for the
   `## Concepts` section and pull design detail into Implementation.
4. **Agent/layer.** Map the work to one agent from the `Agent Workflow.md` roster
   (db / backend / frontend / test / devops / geospatial) → set `agent:`. A ticket
   should stay within **one** layer; if it spans layers, split it into multiple tickets.
5. **Dependencies.** Identify tickets that must finish first → `depends_on:`. If a
   dependency is unmet, set `status: blocked` (or `backlog`).

### 4. Write the body (apply the quality bar)
- **Background** — why this exists; link the feature and concept(s).
- **Implementation** — concrete, **file-path-specific** steps (name the files/dirs,
  e.g. `backend/routes/…`, `frontend/src/lib/api.ts`). One layer. End with an
  explicit **"Do not …"** line (e.g. "Do not add Celery — that's Phase 2"). Keep it
  to one session (≲10 files); if larger, split.
- **Success Criteria** — testable checkboxes. Each line:
  `- [ ] #task [[<Project>]] [[<Assignee>]] <ID>: <objective criterion>`
  (drop the `[[Assignee]]` token if unassigned). Cover the feature's relevant
  acceptance criteria. No vague or placeholder criteria.
- **Concepts** — bullet list of `[[concepts/...]]`.

### 5. Set the frontmatter
Fill every key from the schema in `SKILL.md`:
- `type: ticket`, `id`, `project`, `phase`, `feature`, `agent`, `depends_on`.
- `status` — default `backlog` for captured-for-later, `todo` if it's ready to pick up.
- `priority` — default `medium` unless the user says otherwise.
- `assignee` — `[[T]]`/`[[Ed]]` or blank.
- `created` and `updated` — today's date (`YYYY-MM-DD`).

### 6. Write the file and link it back
- Write to `tickets/<project>/<ID> - <Title>.md`.
- Offer to append `- [[<project>/<ID> - <Title>]]` to the feature file's
  `## Linked Tickets` section (create the section if absent). This keeps the
  feature↔ticket back-link that `sync` checks.

### 7. Confirm + chain
Summarize what you created (ID, title, assignee, phase, feature). Then **offer to
review it** (`review` mode). Don't auto-run review.

## Skeleton

The canonical skeleton is `templates/Ticket Template.md` (symlinked into the vault as
`templates/ticket/Ticket Template.md`). Read it, replace each `<% tp.* %>` placeholder
and HTML comment with the concrete values gathered above, and Write the file. Do not
run `obsidian templater:create-from-template` — that pops interactive dialogs in the
Obsidian UI (it's the manual-creation path; Templater also auto-applies this template
when Ed creates an empty note under `tickets/` by hand).
