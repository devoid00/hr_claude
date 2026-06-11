# Google Calendar Integration

## Setup: google_workspace_mcp

Register at user scope (stored in `~/.claude.json` — MCP servers do **not** go in
`~/.claude/settings.json`):

```sh
claude mcp add --scope user google-workspace \
  -e GOOGLE_OAUTH_CLIENT_ID=<your-client-id> \
  -e GOOGLE_OAUTH_CLIENT_SECRET=<your-client-secret> \
  -- npx -y google_workspace_mcp
```

Equivalent project `.mcp.json` entry:

```json
"google-workspace": {
  "command": "npx",
  "args": ["-y", "google_workspace_mcp"],
  "env": {
    "GOOGLE_OAUTH_CLIENT_ID": "<your-client-id>",
    "GOOGLE_OAUTH_CLIENT_SECRET": "<your-client-secret>"
  }
}
```

To get credentials:
1. Go to Google Cloud Console → APIs & Services → Credentials
2. Create OAuth 2.0 Client ID (Desktop App type)
3. Enable Google Calendar API and Gmail API for the project
4. First run triggers OAuth browser flow — authenticate once, credentials are cached

## Available MCP tools

| Tool | Purpose |
|------|---------|
| `list_calendars` | Get calendar IDs and names |
| `get_events` | Read events in a date range (conflict checking) |
| `manage_event` action=create | Create a new event |
| `manage_event` action=update | Update an existing event |
| `manage_event` action=delete | Delete an event |
| `query_freebusy` | Check availability across calendars |

## Note type → Calendar event mapping

### Transport (flight/train)

```json
{
  "summary": "<carrier> <from>→<to>",
  "start": { "dateTime": "<departure ISO8601>", "timeZone": "<origin timezone>" },
  "end": { "dateTime": "<arrival ISO8601>", "timeZone": "<destination timezone>" },
  "description": "Confirmation: <confirmation>\nSeat: <seat>",
  "colorId": "5"
}
```

### Accommodation (hotel)

```json
{
  "summary": "🏨 <hotel name>",
  "start": { "date": "<checkin YYYY-MM-DD>" },
  "end": { "date": "<checkout YYYY-MM-DD>" },
  "description": "Address: <address>\nConfirmation: <confirmation>\nCost: <cost>",
  "colorId": "3"
}
```

### Activity / Place (timed)

```json
{
  "summary": "<place name>",
  "start": { "dateTime": "<YYYY-MM-DDTHH:MM:00>", "timeZone": "<destination timezone>" },
  "end": { "dateTime": "<YYYY-MM-DDTHH:MM:00>", "timeZone": "<destination timezone>" },
  "description": "Address: <address>\nHours: <hours>\nCost: <cost>",
  "location": "<address>"
}
```

### Activity / Place (all-day, no time set)

```json
{
  "summary": "📍 <place name>",
  "start": { "date": "<YYYY-MM-DD>" },
  "end": { "date": "<YYYY-MM-DD>" }
}
```

## Commit workflow

1. Read all trip notes with `status: planning` (with `--force`, also `confirmed` notes)
2. Group by type: transport → accommodation → places (in that order)
3. Call `list_calendars` — present options, ask user which calendar
4. Call `get_events` for the full trip date range — surface any conflicts
5. For each note (skipping those with `gcal_event_id` already set):
   - Build the event JSON per the mapping above
   - Call `manage_event` action=create
   - Record the returned event ID
6. Update each note's properties:
   ```bash
   obsidian property:set name="status" value="confirmed" path="<path>"
   obsidian property:set name="gcal_event_id" value="<id>" path="<path>"
   ```
7. Report summary: N events created, M skipped (already confirmed), K conflicts flagged

## Re-commit behavior

If a note already has `gcal_event_id`:
- Skip by default (don't duplicate)
- With `--force`: call `manage_event` action=update instead

## Gmail integration

The same `google_workspace_mcp` also exposes Gmail tools. Useful for:
- Drafting booking confirmation requests
- Forwarding confirmation emails to extract confirmation numbers
- Sending itinerary summaries to travel companions
