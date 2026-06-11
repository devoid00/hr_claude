---
aliases:
  - <% tp.file.title %>
type: trip
status: planning
startDate: <% tp.system.prompt("Start date (YYYY-MM-DD)") %>
endDate: <% tp.system.prompt("End date (YYYY-MM-DD)") %>
destination: <% tp.system.prompt("Destination (e.g. Malta)") %>
travelers:
  - Ed
  - <% tp.system.prompt("Second traveler (leave blank if solo)", "") %>
budget:
currency: <% tp.system.prompt("Currency ISO code (e.g. EUR, JPY)") %>
timezone: <% tp.system.prompt("IANA timezone (e.g. Europe/Malta)") %>
cssclasses:
  - wide-note
tags:
  - travel
  - <% tp.system.prompt("Destination tag (e.g. malta)") %>
---

> [!info] Trip at a glance
> **Dates:** <% tp.system.prompt("Start date") %> → <% tp.system.prompt("End date") %>  |  **Travelers:** 2  |  **Budget:**

## Itinerary

> [!tip] Planning
> Open Places.base as a standalone tab → Itinerary view to assign days, Unassigned view to see what's left.

![[travel/<% tp.system.prompt("Vault path (e.g. 2026/Malta)") %>/Places.base#Itinerary]]

## Map

![[travel/<% tp.system.prompt("Vault path (same)") %>/Places.base#Planned Map]]

## Transport

![[travel/<% tp.system.prompt("Vault path (same)") %>/Places.base#Transport]]

## Accommodation

![[travel/<% tp.system.prompt("Vault path (same)") %>/Places.base#Accommodation]]

## Overview

## Entry Requirements

## Budget

## Cultural Notes

## Connectivity

## Resources
