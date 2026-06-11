# Travel Vault Structure

## Folder layout

```
travel/
└── <year>/                              e.g. 2026/
    └── <Destination>/                   e.g. Tokyo/
        ├── Tokyo 2026.md                ← trip hub
        ├── Places.base                  ← single base for all views (places, transport, accommodation)
        ├── Packing.md
        ├── Planning/
        │   ├── Day-1-2026-09-16.md
        │   ├── Day-2-2026-09-17.md
        │   └── ...
        ├── Places/
        │   ├── Senso-ji-Temple.md
        │   └── ...
        └── Logistics/
            ├── Transport/
            │   ├── Flight-SFO-NRT.md
            │   └── Flight-NRT-SFO.md
            └── Accommodation/
                └── Park-Hyatt-Tokyo.md
```

## Naming conventions

- **Hub file**: `<Destination> <Year>.md` → `Tokyo 2026.md`
- **Day files**: `Day-<N>-<YYYY-MM-DD>.md` → `Day-1-2026-09-16.md`
- **Place files**: Title-Case hyphenated → `Senso-ji-Temple.md`, `Tsukiji-Outer-Market.md`
- **Transport files**: `Flight-<FROM>-<TO>.md`, `Train-<Route>.md`
- **Accommodation files**: `Park-Hyatt-Tokyo.md`
- **Year folder**: 4-digit year of trip start date
- **Destination folder**: Title Case, hyphens for spaces → `New-York/`

## Wikilinks

All child notes link to the hub using `trip:` frontmatter:
```yaml
trip: "[[Tokyo 2026]]"
```
This is what `file.hasLink("Tokyo 2026")` filters on. The hub's alias must match exactly.

## Bases files

Always embed using **full vault paths** to avoid ambiguity across multiple trips:
```
![[travel/2026/Tokyo/Places.base#Planned Map]]
![[travel/2026/Tokyo/Places.base#Itinerary]]
![[travel/2026/Tokyo/Places.base#Day Itinerary]]
![[travel/2026/Tokyo/Places.base#Day Map]]
```

### Places.base — 10 views (single base for entire trip)

| View | Type | Filter | Purpose |
|------|------|--------|---------|
| Places | cards | `trip-place` | Candidate review with icons/colors |
| Places List | table | `trip-place` | Sortable list, sorted by day |
| Map | map | `trip-place` | All candidate places on map |
| Planned Map | map | `trip-place` + `day` set | Hub map — only assigned places |
| Itinerary | table | all three types, grouped by `day` | Planning view with count per day — open standalone |
| Unassigned | cards | `trip-place` + `day` empty | Places still needing a day |
| Day Itinerary | table | all three types + `day == this.day` | Embed in day pages |
| Day Map | map | `trip-place` + `day == this.day` | Map embed in day pages |
| Transport | table | `trip-transport` | Hub transport section |
| Accommodation | table | `trip-accommodation` | Hub accommodation section |

**Planning workflow:** Open `Places.base` standalone → **Itinerary** view to assign places
(set `day: 1`, `day: 2` etc.) and see count per day. **Unassigned** view shows what's left.
Embedded bases have limited inline editing — always open standalone for planning sessions.

### mapCenter for Places.base

Set `mapCenter` to the destination's approximate center when creating the base.
Use the centroid of the neighborhoods from the researcher output.
- City trips: `mapZoom: 12`
- Regional trips with day trips: `mapZoom: 10`

## Active trip detection

```bash
obsidian recents | head -5
```
