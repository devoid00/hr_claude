# Mode: promote — hand a mature idea to another skill

Goal: turn a brainstorm/research note into the next artifact and leave links both
ways. This mode doesn't write tickets, decisions, or wiki articles itself — it
invokes the owning skill with good context. Read `SKILL.md` first.

## Procedure

### 1. Load the note
Find the note (by name, or the most recent brainstorm/research note if the user says
"this"). Read it plus its `related:` notes. If important open questions are still
unchecked, say so and ask whether to promote anyway or research first.

### 2. Choose the target
Unless the user already said, ask with AskUserQuestion (multi-select is fine):
- **ticket** — the idea is scoped work for a project that has tickets (Credo, Cookbook).
- **decide** — the next step is choosing between candidates (tools, vendors, approaches).
- **wiki** — some sources or findings are durable general knowledge worth keeping.

### 3. Hand off
- **ticket** → split the idea into one-layer, one-session slices (the ticket skill's
  quality bar). List the slices for the user to confirm, then invoke the `ticket`
  skill in **new** mode once per slice, passing the slice description and a link to
  the source note for its Background section. Mention that the ticket skill offers
  review afterwards.
- **decide** → invoke the `decide` skill in **new** mode, passing the decision
  question and the candidates/criteria found in the note (the Angles section, or
  research comparison tables) so its interview starts pre-filled.
- **wiki** → for each chosen source URL (from `sources:`), invoke the `wiki` skill's
  **ingest**. Don't ingest opinions or project-specific notes, only general knowledge.

### 4. Link both ways
- In the source note: add each new artifact to `promoted_to:`, set `status: promoted`
  (keep `researched` if only wiki ingest happened and the idea itself is still open),
  bump `updated:`.
- In each new ticket / decision hub: make sure it links back to the source note
  (Background for tickets; the hub body for decisions).
- Report what was created, with paths.
