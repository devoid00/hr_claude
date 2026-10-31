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
- Every ticket in `projects/<project>/tickets/` — parse frontmatter + the `#task` checkbox
  lines (`- [ ]`/`- [/]`/`- [x]` containing `#task`).
- `Roadmap.md` phase table → `{phase: status}` (Done / Next / blank…).
- Each `product/use-cases/*.md` (frontmatter `phase`, `status`, `scope`; the
  `## Acceptance criteria` checkboxes) and `product/Requirements.md`.
- `Decisions.md` (which rows are **Decided** vs Undecided).
- `STATUS.md`.
- The team list from `SKILL.md`.

### 2. Run the checks
For each finding decide **AUTO-FIX (safe)** or **FLAG (needs human)**.

| # | Check | Detect | Class |
|---|-------|--------|-------|
| 1 | **Phase-status drift** | ticket's `phase` is **Done** in Roadmap but ticket `status` ≠ `done` | AUTO → set `status: done` |
| 2 | **Coverage gap** | active/next phase has acceptance criteria but **0 tickets**, or a `launch`-scope use case that isn't `built` has no ticket serving it | FLAG → suggest `new` mode (never auto-create) |
| 3 | **Criteria ↔ status** | **all** `#task` boxes checked but `status` ≠ done | AUTO → set `status: done` |
| 3b| (reverse) | `status: done` but unchecked `#task` boxes remain | FLAG (can't assert work is done) |
| 4 | **Decision impact** | a now-**Decided** decision conflicts with ticket content (mentions a rejected option) | FLAG → human review |
| 5 | **Schema validity** | missing `type: ticket`; missing/invalid `status`/`priority`; unknown `assignee` | AUTO add `type`/normalize vocab casing; FLAG unknown assignee |
| 5b| **Broken links** | `serves`/`concept`/`depends_on` link target missing (a link into `_archive/features/` counts as broken) | AUTO if exactly one unambiguous correct target exists; else FLAG |
| 6 | **Use-case status drift** | every ticket serving a use case is `done` but its `status` ≠ `built`, or all its acceptance criteria are ticked but `status` ≠ `built` | FLAG → suggest the status change (shipping is a human call) |
| 7 | **Missing `serves`** | a Credo ticket with no `serves:` (or a leftover `feature:` field) | AUTO rename `feature:`→`serves:` when its target maps unambiguously to a use case; else FLAG |
| 8 | **Staleness** | active-phase ticket with `updated` older than 21 days | FLAG (informational) |

**Safety rules:** never delete a ticket; never invent acceptance criteria or check
a `#task` box (that asserts work was done); when a fix is ambiguous, FLAG instead
of guessing. Categories 2 and 4 are **always** flag-only.

### 3. Apply (unless `--report-only` / `--dry-run`)
Apply the AUTO fixes. Whenever you edit a ticket, set its `updated:` to today.
Optionally refresh `STATUS.md`'s milestone checklist and "Last Updated" date to
match the reconciled ticket states.

### 4. Write the report + summarize
Write `projects/<project>/tickets/_Sync Report.md`:
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
