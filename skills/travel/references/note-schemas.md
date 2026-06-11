# Travel Note Schemas

**The canonical frontmatter and body skeleton for each note type is its template in
`skills/travel/templates/`** (symlinked into the vault as `templates/travel/`). To
create a note, read the template, replace each `<% tp.* %>` placeholder with the
concrete value, and Write the file. Do not duplicate or restyle the structure — if the
structure needs to change, change the template.

| Note type | `type:` | Template | Path in trip |
|-----------|---------|----------|--------------|
| Trip hub | `trip` | `trip-hub.md` | `<Destination> <Year>.md` |
| Day page | `trip-day` | `trip-day.md` | `Planning/Day-<N>-<YYYY-MM-DD>.md` |
| Place | `trip-place` | `trip-place.md` | `Places/<Place-Name>.md` |
| Transport | `trip-transport` | `trip-transport.md` | `Logistics/Transport/<Slug>.md` |
| Accommodation | `trip-accommodation` | `trip-accommodation.md` | `Logistics/Accommodation/<Slug>.md` |
| Packing | (none) | `trip-packing.md` | `Packing.md` |

Templater's file-regex mappings auto-apply these templates when a note is created *by
hand* (empty) in the matching folder; skill-written notes are complete on write and are
not touched.

## Field semantics (what the templates can't express)

**All note types**
- `trip:` is a wikilink back to the hub (`"[[Malta 2026]]"`). This drives Bases
  `file.hasLink(this)` filtering — the hub's alias must match exactly.
- Use `aliases:` not `title:` — Obsidian reserves `title` as an internal property.
- No H1 in note bodies — "Show inline title" is enabled in Obsidian instead.
- Destination tag is the lowercase slug (`malta`, `tokyo`).
- Always use **full vault paths** in `Places.base` embeds (e.g.
  `![[travel/2026/Malta/Places.base#Itinerary]]`) to avoid ambiguity across trips.

**`day:`** (place / transport / accommodation / day page)
- Leave blank on places until the user assigns days in the Itinerary view.
- A place with `day: N` automatically appears in Day N's Itinerary and Day Map embeds
  (both filter on `day == this.day`).
- Transport: set to the departure day number. Accommodation: the check-in day number.

**`coordinates:`** — quoted strings in block sequence format; required for map pins:
```yaml
coordinates:
  - "35.6938"
  - "139.7036"
```

**Place category → icon/color defaults** (the template suggesters offer these; when the
skill fills a place note, pick per this table):

| Category | icon | color |
|---|---|---|
| shrine | landmark | red |
| park | tree-pine | green |
| museum | building-2 | blue |
| market / restaurant | shopping-bag / utensils | orange |
| bar | wine | purple |
| neighborhood | map-pin | gray |
| shopping | shopping-bag | pink |

## Body content guidance

**Hub** (skeleton in the template): fill Overview (3-sentence summary), Entry
Requirements, Budget (per-day budget/mid/splurge + currency + tipping), Cultural Notes,
and Connectivity (eSIM/cash/transit card) from the researcher agent's output; Resources
gets links used during planning. The "Trip at a glance" callout repeats dates/travelers/
budget from frontmatter.

**Day page** (skeleton in the template): the `## Itinerary` embeds auto-populate — never
add a hardcoded places list. The `day` operation fills Meals and Notes with the timed
schedule. Bookable items use Tasks plugin syntax: `- [ ] Reserve at X 📅 YYYY-MM-DD`.

**Packing**: Tasks plugin checkboxes; sections Documents / Electronics / Clothing /
Destination-specific (skeleton in the template).
