# Operations: add / update — manage candidates

Read `SKILL.md` first. Identify the target decision (explicit, or the only active one,
or ask). The decision's schema is the Criteria table in its hub note + the generated
template at `templates/decide/<slug>-candidate.md` — read both before writing.

## add `<name or url> [to <decision>]`

1. **If given a URL** (listing, posting, product page):
   - Fetch with `obsidian:defuddle`. Listing sites (Zillow etc.) often block scraping —
     this is **best-effort**: extract whatever fact criteria the page yields (price,
     bedrooms, company, role, …) and ask the user for the rest. Never guess values;
     leave unknown facts blank and say which.
   - Derive the candidate name (address, "Company — Role", product name).
   - Set `url:` to the source.
2. **If given a name/description**: parse any facts from the message
   (`add "14 Foo St" $2400 2bd` → `price: 2400`, `bedrooms: 2`); ask for facts that are
   cheap to provide now; leave the rest blank.
3. If the schema has `coordinates` and an address is known, WebSearch the coordinates
   (quoted block-sequence format).
4. Fill the candidate template → Write
   `decisions/<Decision Name>/candidates/<Candidate Name>.md`.
   `status:` = first status in the flow; `added:` = today; ratings blank.
5. Confirm: name, filled facts, missing facts. The note appears in the Base views
   automatically.

## update `<candidate>`

For after a viewing / interview / research session.

1. Read the candidate note.
2. Fill in what the user reports: missing facts, the `rating_*` values (1–5 integers —
   if the user gives prose, propose numbers and confirm), corrections.
3. Move `status` along the decision's flow when the event implies it (a viewing →
   `visited`; "I applied" → `applied`; "they made an offer" → `offer`). Don't skip to
   the terminal status — that's `decide`'s job.
4. Append a dated entry under `## Notes` with the user's qualitative impressions —
   ratings compress; the prose is what `compare` quotes later.
5. If all rated criteria are now filled, mention the candidate is fully scored.
