---
aliases:
  - <% tp.file.title %>
type: trip-place
trip: "[[<% tp.system.prompt("Trip name (e.g. Malta 2026)") %>]]"
category: <% tp.system.suggester(["neighborhood", "restaurant", "museum", "shrine", "activity", "market", "bar", "park", "shopping"], ["neighborhood", "restaurant", "museum", "shrine", "activity", "market", "bar", "park", "shopping"], false, "Category") %>
icon: "<% tp.system.suggester(["map-pin", "utensils", "building-2", "landmark", "tree-pine", "shopping-bag", "wine"], ["map-pin", "utensils", "building-2", "landmark", "tree-pine", "shopping-bag", "wine"], false, "Icon") %>"
color: "<% tp.system.suggester(["gray", "orange", "blue", "red", "green", "purple", "pink"], ["gray", "orange", "blue", "red", "green", "purple", "pink"], false, "Color") %>"
address: <% tp.system.prompt("Address") %>
coordinates:
  - "<% tp.system.prompt("Latitude") %>"
  - "<% tp.system.prompt("Longitude") %>"
hours: <% tp.system.prompt("Hours (e.g. 09:00–17:00 daily)", "hours TBC") %>
cost: <% tp.system.prompt("Cost (e.g. free, €10 adults)", "free") %>
url: "<% tp.system.prompt("URL (leave blank if none)", "") %>"
day:
tags:
  - travel
  - <% tp.system.prompt("Destination tag (e.g. malta)") %>
  - places
---

<% tp.system.prompt("One-paragraph description") %>

## Links
- [Wikipedia]()
