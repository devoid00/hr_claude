# Mode: explore — brainstorm an idea

Goal: turn a fuzzy idea into one brainstorm note that widens out the options and
ends with clear open questions and next steps. Read `SKILL.md` first for paths,
schema, and writing rules.

## Procedure

### 1. Place it
- Work out the **project** (or none). If unclear, ask with AskUserQuestion: the
  likely project(s) plus "No project".
- **Check for duplicates**: Grep the vault (`projects/`, `brainstorm/`, the
  inboxes, `projects/Brainstorm.md`, `projects/claude/Ideas.md`) for the idea's key
  terms. If a note already exists, offer to extend it rather than start a new one.
  If the idea came from an inbox bullet, remember it so you can link it in step 5.

### 2. Short interview (one round)
Ask at most 3–4 questions in a single AskUserQuestion call, and skip anything the
user already said. Pick from:
- **Goal** — what would make this worth doing? Who is it for?
- **Constraints** — time, money, tech stack, things it must or must not touch.
- **Success** — what would "it worked" look like?
- **Appetite** — weekend hack, side project, or a real feature of <project>?

If the user wants to skip questions ("just go"), skip them and note your
assumptions in the Idea section.

### 3. Widen out
Before writing, think broadly. Ground it in the vault: for a project idea, skim the
project's main doc (`projects/<p>/<P>.md`, plus `Roadmap.md` and `Decisions.md` if they
exist) so the angles respect what's already decided. Cover:
- **Angles** — 3–5 genuinely different approaches, each with a one-line upside/downside.
- **Prior art** — existing tools/projects/papers worth checking. Light lookups
  (a WebSearch or two) are fine; deep reading is research mode's job.
- **Risks / unknowns** — what could make this not work.
- **Cheapest test** — the smallest thing that would prove or kill the idea.

### 4. Write the note
Fill `templates/Brainstorm Template.md` (see `SKILL.md` for the path rule):
`type: brainstorm`, `status: exploring`, sources from any lookups. Sections:
*Idea*, *Why*, *Angles*, *Prior art*, *Risks / unknowns*, *Open questions*
(checkboxes), *Next steps*. Put your own recommendation in a `> [!note] Take` callout
under Angles. Create `brainstorm/Brainstorm.base` if it's missing (see `SKILL.md`).

### 5. Link and offer
- If it came from an inbox bullet, change that bullet to link the new note.
- Add `related:` links to existing notes you found in step 1 or 3.
- Offer: "Want me to research the open questions?" (→ research mode, passing this note).
