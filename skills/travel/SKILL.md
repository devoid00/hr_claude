---
name: travel
description: >-
  Plan, organize, and commit travel trips using Obsidian notes (vault at
  /Users/ed/Documents/notes/travel/) and Google Calendar. Operations: plan a
  destination ("plan a trip to", "I want to go to", "plan Tokyo"); day-plan to
  generate day pages ("generate day pages", "build the itinerary"); add a
  flight/hotel/place/activity ("add a flight", "I'm staying at", "flying to");
  day <n> to flesh out a day's schedule ("plan day 1", "what should I do on
  day 2"); commit confirmed items to Google Calendar ("commit the trip", "add
  to calendar", "finalize"); query existing trips ("what's my itinerary",
  "when does my flight", "what hotels did I book").
argument-hint: plan <destination> [dates] [travelers] | day-plan | add <flight|hotel|place|activity> | day <n> | commit [<trip>] [--dry-run|--force] | query <question>
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Skill, Agent, AskUserQuestion, mcp__google-workspace
---

# Travel Skill

Plans and organizes trips in the Obsidian vault with Google Calendar integration.
See `references/note-schemas.md` for field semantics and `references/vault-structure.md`
for folder layout and Places.base view reference.

**Creating notes:** the canonical structure for every note type is its template in
`templates/` (symlinked into the vault as `templates/travel/`). Read the template,
replace each `<% tp.* %>` placeholder with the concrete value, and Write the note —
never run `obsidian templater:create-from-template` from the skill (it pops interactive
dialogs in the Obsidian UI; that command is the *manual* path for Ed).

Plugin skills to invoke when needed:
- `obsidian:defuddle` — fetch clean markdown from travel guides and destination URLs
- `obsidian:obsidian-cli` — vault search, note creation, property updates
- `obsidian:obsidian-markdown` — correct frontmatter, wikilinks, callout syntax
- `obsidian:obsidian-bases` — create/edit Bases views

Installed Obsidian plugins: dataview, obsidian-map-view, obsidian-tasks-plugin,
obsidian-meta-bind-plugin. Maps use the built-in Bases map view (the Map View plugin
is required for the `map-view-plugin` type). Google Calendar goes through the
`google-workspace` MCP server — see `references/calendar.md` for setup.
Vault root: `/Users/ed/Documents/notes`.

Active trips (hub notes with `type: trip`):
!`grep -rl --include="*.md" --exclude-dir=_templates "^type: trip$" /Users/ed/Documents/notes/travel 2>/dev/null | sort`

---

## plan `<destination> [dates] [travelers]`

Research a destination and create the trip structure. Day pages come after review.

**Step 1 — Research**

Launch a general-purpose subagent (Agent tool) whose prompt is the full contents of
`agents/researcher.md` followed by the inputs: destination name and country, travel
dates, number of travelers.

Returns structured markdown: overview, entry requirements, key neighborhoods with
coordinates, logistics, budget, suggested pacing, cultural notes, connectivity.

**Step 2 — Create vault structure and hub**

Create folders (no day pages yet):
```
travel/<year>/<Destination>/
├── Planning/
├── Places/
└── Logistics/
    ├── Transport/
    └── Accommodation/
```

Write `travel/<year>/<Destination>/<Destination> <Year>.md` — main trip hub — by
filling `templates/trip-hub.md` (body content guidance in `references/note-schemas.md`).

**Step 3 — Create Places.base**

Create `travel/<year>/<Destination>/Places.base` with all 10 views following the
template in `references/vault-structure.md`. Set `mapCenter` to the destination's
approximate center (centroid of key neighborhoods from researcher output).
- City trips: `mapZoom: 12`
- Regional trips with day trips: `mapZoom: 10`

Always use **full vault paths** in all embeds.

**Step 4 — Generate place candidates**

Create 20-30 Place notes in `Places/` covering: temples/shrines, neighborhoods,
restaurants, museums, viewpoints, markets, bars/nightlife, and day-trip sites.

Fill `templates/trip-place.md` for each, per the semantics in
`references/note-schemas.md`:
- `coordinates:` as quoted block sequence (required for map pins)
- `icon:` and `color:` per the category defaults table
- `tags: [travel, <slug>, places]`
- `day:` left blank

**Step 5 — Stop and present for review**

Tell the user:
- N place candidates created — review in Places.base (Places cards / Map views)
- Delete unwanted places, add new ones with `/travel add place`
- Assign `day:` numbers in Places.base → Itinerary view (open standalone)
- Run `/travel day-plan` once days are assigned to generate day pages

---

## day-plan `[<trip>]`

Generate day pages from Place notes that have `day:` assigned. Run after review.

1. Read all Place notes where `day:` is not blank; group by day number
2. Detect trip dates from hub frontmatter to map day numbers to calendar dates
3. For each day, write `Planning/Day-<N>-<YYYY-MM-DD>.md` by filling
   `templates/trip-day.md` (semantics in `references/note-schemas.md`)
4. Write `Packing.md` from `templates/trip-packing.md`

If some places still have `day:` blank, ask whether to skip them or assign them first.

Day pages do NOT contain a hardcoded places list — the `## Itinerary` section embeds
`Places.base#Day Itinerary` which auto-populates from Place notes with matching `day:`.

---

## add `<flight|hotel|place|activity> [details]`

Add a single item to the active trip.

**Detect active trip:**
```bash
obsidian recents | head -5
```

**File paths and templates by type:**
- Flight/train: `Logistics/Transport/<Slug>.md` ← `templates/trip-transport.md`
- Hotel: `Logistics/Accommodation/<Slug>.md` ← `templates/trip-accommodation.md`
- Place/activity: `Places/<Slug>.md` ← `templates/trip-place.md`

**Parse natural language into the template's frontmatter:**
- `add flight SFO→NRT Sep 14 23:00 arrival Sep 16 05:30` →
  `subtype: flight, from: SFO, to: NRT, departure: 2026-09-14T23:00, arrival: 2026-09-16T05:30, day: 1`
- `add hotel "Park Hyatt Tokyo" Sep 15-24` →
  `type: trip-accommodation, checkin: 2026-09-15, checkout: 2026-09-24, day: 1`
- `add place "Tsukiji Market" restaurant` →
  `type: trip-place, category: restaurant` + WebSearch for coordinates

For transport and accommodation: set `day:` to the departure/checkin day number so they
appear in the Day Itinerary view on the correct day page.

For places: set `icon:` and `color:` per category defaults in `references/note-schemas.md`.

---

## day `<n> [for <trip>]`

Flesh out a day page with a timed schedule.

1. Read the day page (`Planning/Day-N-<date>.md`)
2. Read all Place notes assigned to that day (`day: N`)
3. WebSearch opening hours and travel times between places
4. Write a timed schedule into the **Meals** and **Notes** sections:
   - Morning / lunch / afternoon / dinner / evening blocks
   - Travel time estimates between places
   - Tasks plugin syntax for bookable items: `- [ ] Reserve at X 📅 YYYY-MM-DD`

Do NOT add a hardcoded places list — the `## Itinerary` embed handles that automatically.

---

## commit `[<trip>]`

Push confirmed trip items to Google Calendar.

**Requires:** the `google-workspace` MCP server. Setup, event mapping, and the full
workflow live in `references/calendar.md` — follow that file; summary:

1. Read all notes in the trip where `status: planning`; skip notes that already
   have `gcal_event_id`
2. Use `list_calendars` — ask user which calendar to write to
3. Use `get_events` to check for conflicts on the trip dates
4. Create calendar events via `manage_event` per the mapping in `references/calendar.md`
5. Update each note after successful creation:
   ```bash
   obsidian property:set name="status" value="confirmed" path="<note-path>"
   obsidian property:set name="gcal_event_id" value="<event-id>" path="<note-path>"
   ```
6. Report: N events created, M conflicts flagged, K skipped (already confirmed)

Flags: `--dry-run` prints proposed events without writing to Google Calendar;
`--force` also re-processes notes with `gcal_event_id`, updating their events.

---

## query `<question>`

Answer questions about existing trips from vault notes.

```bash
obsidian search:context query="<keywords>" path="travel" limit=10
```

Read the top matching notes and synthesize. Cite note names. For itinerary questions,
read the relevant day pages and their assigned Place notes directly.

---

## Reference files

| File | Contents |
|------|----------|
| `references/note-schemas.md` | Frontmatter schemas + hub/day body templates |
| `references/vault-structure.md` | Folder layout, Places.base views reference |
| `references/calendar.md` | google_workspace_mcp setup + event mapping |

## Agents

| Agent | Purpose |
|-------|---------|
| `agents/researcher.md` | Destination research: visa, neighborhoods, logistics, budget. Launched as a general-purpose subagent in `plan` Step 1. |
