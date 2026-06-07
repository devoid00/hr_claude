---
description: Create, review, or sync Credo / vault project tickets
argument-hint: new | review | sync
allowed-tools: Skill, AskUserQuestion, Read, Write, Edit, Bash, Glob, Grep
---
Handle the project-ticket request using the `ticket` skill. Arguments: "$ARGUMENTS".

1. Determine the mode:
   - If "$ARGUMENTS" starts with or clearly implies `new`, `review`, or `sync`, use that mode.
   - Otherwise (empty or ambiguous), call the **AskUserQuestion** tool with a single question
     ("Which ticket action?") offering these options:
     - **new** — draft a new ticket
     - **review** — review an existing ticket
     - **sync** — validate tickets against the design docs
2. Invoke the `ticket` skill and follow its instructions for the chosen mode, passing along
   any remaining text in "$ARGUMENTS" (e.g. a ticket description, an ID, or `--report-only`).
