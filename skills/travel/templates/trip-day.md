---
aliases:
  - "Day <% tp.system.prompt("Day number") %> — <% tp.system.prompt("Theme / focus area") %>"
type: trip-day
day: <% tp.system.prompt("Day number") %>
trip: "[[<% tp.system.prompt("Trip name (e.g. Malta 2026)") %>]]"
date: <% tp.system.prompt("Date (YYYY-MM-DD)") %>
theme: <% tp.system.prompt("Theme / focus area (e.g. Valletta)") %>
tags:
  - travel
  - <% tp.system.prompt("Destination tag (e.g. malta)") %>
---

> [!info] Focus
> <% tp.system.prompt("One-line focus for the day") %>

## Itinerary

![[travel/<% tp.system.prompt("Vault path (e.g. 2026/Malta)") %>/Places.base#Day Itinerary]]

![[travel/<% tp.system.prompt("Vault path (same as above)") %>/Places.base#Day Map]]

## Meals
- **Breakfast:**
- **Lunch:**
- **Dinner:**

## Notes

## Tasks
