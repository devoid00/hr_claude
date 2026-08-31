# Operations: compare / decide — verdicts

Read `SKILL.md` first. Identify the target decision.

## compare `[<decision>]`

1. Read the hub (criteria, weights, status flow) and **every** candidate note —
   frontmatter and `## Notes` prose.
2. Build the comparison:
   - Ranked table (by score when weighted; otherwise by your judgment with the ranking
     basis stated): candidate, status, key facts, score.
   - **Tradeoffs in prose** — what each top candidate wins and loses on, quoting the
     user's own notes where they're decisive.
   - **Missing data** — unfilled facts/ratings that could change the ranking; suggest
     `update` for those candidates.
   - Exclude `rejected` candidates from the ranking (list them in one line at the end).
3. Rewrite the hub's `## Verdict` section (replace its content, keep the heading):
   date, the ranked table, 2–3 sentences of recommendation, open questions.
4. **Never auto-decide.** End by asking whether to record a choice (`decide`) or what
   data to gather next.

## decide `<candidate>`

1. Confirm the choice if there's any ambiguity — this closes the decision.
2. Candidate note: `status:` → the flow's terminal status (`chosen` / `accepted` / …).
3. Hub: `status: decided`, `chosen: "[[<Candidate>]]"`, and append a dated one-liner to
   `## Verdict` recording the decision and the main reason.
4. Ask whether to mark the remaining non-rejected candidates `rejected` (don't do it
   unprompted — a job search may keep a backup offer alive).
5. The decision drops out of `Decisions.base → Open` automatically via `status`.
