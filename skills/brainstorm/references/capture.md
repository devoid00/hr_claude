# Mode: capture — quick idea dump

Goal: get the idea written down in seconds. No research, no interview. Read
`SKILL.md` for paths.

## Procedure

1. **Pick the inbox.** If the user names a project (or it's obvious), use
   `projects/<project>/research/Inbox.md`; otherwise `brainstorm/Inbox.md`. Only
   ask if it's truly ambiguous.
2. **Create the inbox if missing** with this header:
   ```markdown
   ---
   type: inbox
   ---
   # Ideas Inbox

   Quick captures. Run `/hr:brainstorm explore` on one to flesh it out.

   ```
3. **Append** one bullet per idea, newest at the bottom:
   ```markdown
   - 2026-11-09 — QR codes on storage bins, look-up in Obsidian #idea
   	- any sub-bullets the user gave, verbatim
   ```
   Keep the user's wording; fix only obvious typos.
4. **Legacy list (first run only).** If `brainstorm/Inbox.md` was just created and
   `projects/Brainstorm.md` exists, ask once whether to copy its items into the new
   inbox. Copy only — never move, edit, or delete `projects/Brainstorm.md` (or
   `projects/claude/Ideas.md`) unless the user explicitly says so.
5. **Confirm** in one line (inbox path) and offer: "Want to explore this one now?"
