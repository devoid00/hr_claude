---
aliases:
  - <% tp.file.title %>
type: trip-accommodation
trip: "[[<% tp.system.prompt("Trip name (e.g. Malta 2026)") %>]]"
address: <% tp.system.prompt("Full address") %>
coordinates:
  - "<% tp.system.prompt("Latitude") %>"
  - "<% tp.system.prompt("Longitude") %>"
checkin: <% tp.system.prompt("Check-in date (YYYY-MM-DD)") %>
checkout: <% tp.system.prompt("Check-out date (YYYY-MM-DD)") %>
confirmation: <% tp.system.prompt("Booking reference") %>
cost: <% tp.system.prompt("Cost (total or per night)") %>
url: "<% tp.system.prompt("URL (booking page or hotel site)", "") %>"
day: <% tp.system.prompt("Day number (check-in day)") %>
tags:
  - travel
  - <% tp.system.prompt("Destination tag (e.g. malta)") %>
---
