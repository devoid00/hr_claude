# Operation: new — interview + scaffold a decision workspace

Goal: agree a criteria schema with the user, then create the full workspace. Read
`SKILL.md` first for layout and frontmatter rules.

## 1. Interview

1. Establish **decision name** (folder/hub name, e.g. `Housing 2026`) and **domain**.
2. Open `references/domains.md`. If a pack matches, present its criteria table and
   status flow as the starting point; otherwise build from scratch (facts that
   distinguish options / qualities to rate / stages candidates move through).
3. Let the user cut, add, and rename criteria. Keys: lowercase snake_case; ratings
   prefixed `rating_` (integer 1–5).
4. **Weights (optional).** Ask whether the rated criteria should produce a score
   column; if yes, get a weight (integer, e.g. 1–5) per rated criterion. No weights →
   no score column; the table is still sortable per column.
5. Confirm the status flow (pack default unless edited) and the terminal status.

## 2. Scaffold

Create:
```
decisions/<Decision Name>/
├── <Decision Name>.md      ← fill skills/decide/templates/decision-hub.md
└── candidates/             ← empty
```
- Hub: fill the template's `<% tp.* %>` placeholders; complete the **Criteria** table
  (one row per criterion: name, key, kind, weight-or-blank) and the **Status flow**
  line. This table is the human-readable record of the schema — keep it exact.
- Generate the candidate template at
  `templates/decide/<decision-slug>-candidate.md` (real vault folder). Style it like
  the existing skill templates — `<% tp.system.prompt %>` per fact,
  `<% tp.system.suggester %>` for status — e.g.:

```markdown
---
aliases:
  - <% tp.file.title %>
type: decision-candidate
decision: "[[<Decision Name>]]"
status: <first status in the flow>
url: "<% tp.system.prompt("Listing/posting URL (blank if none)", "") %>"
added: <% tp.date.now("YYYY-MM-DD") %>
<fact key>: <% tp.system.prompt("<Fact name>") %>
…
rating_<name>:
…
---

<% tp.system.prompt("One-line first impression") %>

## Notes
```
  Ratings are left blank in the template (filled by `update` after seeing the
  candidate). Coordinates, if present, use the quoted block-sequence format
  (travel pattern).

## 3. Generate `Candidates.base`

Write `decisions/<Decision Name>/Candidates.base`. Use the `obsidian:obsidian-bases`
skill for syntax; the working patterns in this vault are `tickets/Tickets.base`
(formulas, groupBy boards) and `travel/2026/Malta/Places.base` (map views).

Skeleton:
```yaml
filters:
  and:
    - type == "decision-candidate"
    - file.hasLink("<Decision Name>")
formulas:                       # only when weights were chosen
  score: '((rating_a * Wa) + (rating_b * Wb)) / (Wa + Wb)'
properties:
  <each key>:
    displayName: <Criterion name>
  formula.score:
    displayName: Score
views:
  - type: table
    name: Table                 # every criterion column; sort by formula.score DESC when scored
  - type: cards
    name: Cards                 # file.name + the 4–5 most decision-relevant columns
  - type: table
    name: Shortlist             # filters: status == "<shortlist-ish status>"
  - type: table
    name: Board                 # groupBy: property: status (pipeline tracker)
  - type: map-view-plugin       # ONLY if the schema has coordinates
    name: Map                   # mapCenter from the candidates' area, mapZoom 12
```
Bake the agreed weights into the `score` formula and document them in the hub's
Criteria table — they must match.

## 4. Templater auto-apply mapping

Append to `file_templates` in
`/Users/ed/Documents/notes/.obsidian/plugins/templater-obsidian/data.json`:
```json
{
  "regex": "decisions/<Decision Name>/candidates/.*",
  "template": "templates/decide/<decision-slug>-candidate.md"
}
```
then `obsidian plugin:reload id=templater-obsidian`. (Templater only auto-applies to
empty files, so skill-written candidates are unaffected.)

## 5. `decisions/Decisions.base` (first run only)

If absent, create the overview base:
```yaml
filters:
  and:
    - type == "decision"
views:
  - type: table
    name: All
    order: [file.name, domain, status, created, chosen]
  - type: table
    name: Open
    filters:
      and:
        - status == "deciding"
```

## 6. Present and stop

Summarize: workspace path, criteria + weights, status flow. Tell the user to add
candidates with `/decide add <name or url>` — don't invent candidates yourself.
