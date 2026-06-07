# Mode: review — review a ticket

Goal: grade one ticket against a rubric and propose concrete fixes. Read `SKILL.md`
first for the schema and the quality bar.

## Procedure

### 1. Identify and load
- Identify the target ticket (by ID, filename, or "this ticket"). If unclear, ask.
- Read the ticket file.
- For grounding, read its linked `feature` file and `Decisions.md` (and any
  `concepts/*.md` it references or should).

### 2. Grade the rubric
Mark each item ✅ pass / ⚠️ warn / ❌ fail, with a one-line reason citing the
ticket. Items derive from `Agent Workflow.md` + the schema.

**Frontmatter & metadata**
- All schema keys present; `type: ticket`.
- `status` and `priority` are in the allowed vocab.
- `assignee` is `[[T]]`/`[[Ed]]` or intentionally blank (flag unknown people).
- `id` matches the filename prefix.
- `feature` link resolves; `phase` matches that feature's phase.
- `depends_on` links all resolve.

**Background**
- States *why* the ticket exists (not just what).
- Links the relevant concept(s) and the feature.

**Implementation**
- Concrete and **file-path-specific** (names actual files/dirs).
- Scoped to **one** layer/agent (matches `agent:`).
- Has an explicit **"Do not …"** boundary.
- Completable in one session (≲10 files); flag if it looks larger → suggest splitting.

**Success Criteria**
- Every criterion is **testable/objective** (flag vague or placeholder text, e.g. junk like `lsakjfd`, "works", "looks good").
- Each has a `#task [[<Project>]] … <ID>:` line with the correct project tag and ID.
- Criteria cover the feature's relevant acceptance criteria (note any gaps).

**Consistency**
- Honors **decided** decisions in `Decisions.md` (flag any reliance on a rejected option).
- All wikilinks resolve.

### 3. Report
Output two parts:
1. A compact table: `Item | Verdict | Note`.
2. A prioritized list of **concrete suggested edits** (the exact wording/paths to
   change), most important first. Give an overall verdict (ready / needs work).

### 4. Offer to apply
Offer to apply the suggested edits. Only edit on confirmation. When you do, bump
`updated:` to today. Do not change `status` as part of a review unless the user asks.
