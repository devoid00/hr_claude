---
type: ticket
id: <% tp.system.prompt("Ticket ID (e.g. C006)") %>
project: <% tp.system.suggester(["Credo", "Cookbook"], ["Credo", "Cookbook"], false, "Project") %>
phase: <% tp.system.prompt("Phase number") %>
status: <% tp.system.suggester(["backlog", "todo", "in-progress", "blocked", "review", "done"], ["backlog", "todo", "in-progress", "blocked", "review", "done"], false, "Status") %>
priority: <% tp.system.suggester(["high", "medium", "low"], ["high", "medium", "low"], false, "Priority") %>
assignee: "<% tp.system.suggester(["[[T]]", "[[Ed]]", "(unassigned)"], ["[[T]]", "[[Ed]]", ""], false, "Assignee") %>"
feature: "<% tp.system.prompt("Feature link (e.g. [[projects/credo/features/Phase 2 - Core Views]])") %>"
depends_on: []
agent: <% tp.system.suggester(["db", "backend", "frontend", "test", "devops", "geospatial", "(none)"], ["db", "backend", "frontend", "test", "devops", "geospatial", ""], false, "Agent / layer") %>
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
---
# <% tp.file.title %>

## Background
<!-- Why this ticket exists. Link the [[feature]] and the relevant [[concepts/...]]. -->

## Implementation
<!-- Concrete, file-path-specific steps. One layer only. -->
- 
- Do not <!-- explicit out-of-scope boundary, e.g. "do not add Celery — that's Phase 2" -->.

## Success Criteria
<!-- Replace ID + assignee. Each criterion must be objective/testable. -->
- [ ] #task [[Credo]] ID: <!-- add [[T]] or [[Ed]] after the project link to assign -->
- [ ] #task [[Credo]] ID: 

## Concepts
- 
