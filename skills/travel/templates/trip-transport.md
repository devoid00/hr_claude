---
aliases:
  - <% tp.file.title %>
type: trip-transport
trip: "[[<% tp.system.prompt("Trip name (e.g. Malta 2026)") %>]]"
subtype: <% tp.system.suggester(["flight", "train", "bus", "ferry", "taxi", "car"], ["flight", "train", "bus", "ferry", "taxi", "car"], false, "Transport type") %>
from: <% tp.system.prompt("From (e.g. LHR, London)") %>
to: <% tp.system.prompt("To (e.g. MLA, Malta)") %>
departure: <% tp.system.prompt("Departure (YYYY-MM-DDTHH:MM)") %>
arrival: <% tp.system.prompt("Arrival (YYYY-MM-DDTHH:MM)") %>
carrier: <% tp.system.prompt("Carrier / operator") %>
confirmation: <% tp.system.prompt("Booking reference") %>
cost: <% tp.system.prompt("Cost") %>
seat: <% tp.system.prompt("Seat (optional)", "") %>
day: <% tp.system.prompt("Day number") %>
tags:
  - travel
  - <% tp.system.prompt("Destination tag (e.g. malta)") %>
---
