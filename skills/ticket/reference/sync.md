# Mode: sync — validate tickets against design docs

Goal: reconcile a project's tickets with its design docs. **Default behavior:
auto-fix the safe, unambiguous changes and report everything that needs a human
decision.** Read `SKILL.md` first.

## Flags
- *(default)* — apply safe fixes, write a report of the rest.
- `--report-only` — never edit anything; only produce the report.
- `--dry-run` — show the edits you *would* make, but don't write them.

## Procedure

### 1. Load state (read-only first)
For the project (default Credo):
- Every ticket in `tickets/<project>/` — parse frontmatter + the `#task` checkbox
  lines (`- [ ]`/`- [/]`/`- [x]` containing `#task`).
- `Roadmap.md` phase table → `{phase: status}` (Done / Next / blank…).
- `features/Features.md` table + each `features/Phase N - *.md` (frontmatter `status`,
  the `## Status` body line, acceptance-criteria checkboxes, `## Linked Tickets`).
- `Decisions.md` (which rows are **Decided** vs Undecided).
- `STATUS.md`.
- The team list from `SKILL.md`.

### 2. Run the checks
For each finding decide **AUTO-FIX (safe)** or **FLAG (needs human)**.

| # | Check | Detect | Class |
|---|-------|--------|-------|
| 1 | **Phase-status drift** | ticket's `phase` is **Done** in Roadmap/Features but ticket `status` ≠ `done` | AUTO → set `status: done` |
| 2 | **Coverage gap** | active/next phase has acceptance criteria but **0 tickets**, or feature criteria not covered by any ticket success-criterion | FLAG → suggest `new` mode (never auto-create) |
| 3 | **Criteria ↔ status** | **all** `#task` boxes checked but `status` ≠ done | AUTO → set `status: done` |
| 3b| (reverse) | `status: done` but unchecked `#task` boxes remain | FLAG (can't assert work is done) |
| 4 | **Decision impact** | a now-**Decided** decision conflicts with ticket content (mentions a rejected option) | FLAG → human review |
| 5 | **Schema validity** | missing `type: ticket`; missing/invalid `status`/`priority`; unknown `assignee` | AUTO add `type`/normalize vocab casing; FLAG unknown assignee |
| 5b| **Broken links** | `feature`/`concept`/`depends_on` link target missing | AUTO if exactly one unambiguous correct target exists; else FLAG |
| 6 | **Feature-status drift** | feature frontmatter `status` ≠ `Features.md` ≠ `Roadmap.md`, or the duplicated `## Status` body line disagrees with frontmatter | AUTO when one source is unambiguously authoritative (Roadmap/Features agree); else FLAG |
| 7 | **Back-link integrity** | feature `## Linked Tickets` ↔ tickets' `feature` field disagree | AUTO add the missing back-link |
| 8 | **Staleness** | active-phase ticket with `updated` older than 21 days | FLAG (informational) |

**Safety rules:** never delete a ticket; never invent acceptance criteria or check
a `#task` box (that asserts work was done); when a fix is ambiguous, FLAG instead
of guessing. Categories 2 and 4 are **always** flag-only.

### 3. Apply (unless `--report-only` / `--dry-run`)
Apply the AUTO fixes. Whenever you edit a ticket, set its `updated:` to today.
Optionally refresh `STATUS.md`'s milestone checklist and "Last Updated" date to
match the reconciled ticket states.

### 4. Write the report + summarize
Write `tickets/<project>/_Sync Report.md`:
```markdown
# <Project> — Sync Report (<date>)

## Summary
- Tickets scanned: N   ·   Auto-fixed: A   ·   Needs review: F

## Auto-fixed
- C00X — set status backlog → done (phase 1 is Done in Roadmap)
- …

## Needs review
- Phase 2 has acceptance criteria but no tickets → run `ticket` new for: <list>
- C00Y mentions Mapbox, but "Map library" is Decided: Leaflet → update or confirm
- …
```
Then give a short chat summary and, if there are coverage gaps, offer to switch to
`new` mode to draft the missing tickets.
