# Domain starter packs

Starter criteria sets for the `new` interview. They are **starting points** — present
the pack, let the user cut/add/rename criteria and adjust the status flow, then
scaffold from the final list. For a domain with no pack, build the list from scratch
in conversation (ask: what facts distinguish the options? what qualities will you rate
after seeing them? what stages does a candidate move through?).

Criterion kinds:
- **fact** — number, text, date, or coordinates; filled at add-time (from a URL or the
  user) and sortable in the Base.
- **rating** — integer 1–5, key prefixed `rating_`; usually filled by `update` after a
  visit/interview/research; eligible for a weight in the score formula.

Including a `coordinates` criterion ⇒ the Base gets Map views (travel pattern).

---

## housing

| Criterion | Key | Kind |
|---|---|---|
| Price (rent or sale) | `price` | fact (number) |
| Bedrooms | `bedrooms` | fact (number) |
| Bathrooms | `bathrooms` | fact (number) |
| Size (sqft) | `sqft` | fact (number) |
| Neighborhood | `neighborhood` | fact (text) |
| Address | `address` | fact (text) |
| Coordinates | `coordinates` | fact (coordinates) |
| Commute (minutes) | `commute_min` | fact (number) |
| Available from | `available` | fact (date) |
| Location quality | `rating_location` | rating |
| Condition / finish | `rating_condition` | rating |
| Light & noise | `rating_light_noise` | rating |
| Value for money | `rating_value` | rating |

**Status flow:** `candidate → shortlist → visited → chosen / rejected`
**Terminal:** `chosen`

## job-search

| Criterion | Key | Kind |
|---|---|---|
| Company | `company` | fact (text) |
| Role / title | `role` | fact (text) |
| Salary (or range) | `salary` | fact (text) |
| Location / remote | `location` | fact (text) |
| Posting URL | `url` | fact (already in base schema) |
| Applied date | `applied` | fact (date) |
| Culture fit | `rating_culture` | rating |
| Growth / learning | `rating_growth` | rating |
| Compensation | `rating_comp` | rating |
| Interest in the work | `rating_interest` | rating |

**Status flow:** `interested → applied → interviewing → offer → accepted / rejected`
(`rejected` covers both directions — note which in the body)
**Terminal:** `accepted`

## purchase (laptop, appliance, gear…)

| Criterion | Key | Kind |
|---|---|---|
| Price | `price` | fact (number) |
| Brand / model | `model` | fact (text) |
| Key spec 1–3 | per item (e.g. `ram_gb`, `weight_kg`) | fact |
| Reviews summary | `rating_reviews` | rating |
| Fit for purpose | `rating_fit` | rating |

**Status flow:** `candidate → shortlist → chosen / rejected`
**Terminal:** `chosen`

## vehicle

| Criterion | Key | Kind |
|---|---|---|
| Price | `price` | fact (number) |
| Make / model / year | `model` | fact (text) |
| Mileage | `mileage` | fact (number) |
| Fuel / range | `fuel` | fact (text) |
| Condition | `rating_condition` | rating |
| Drive feel | `rating_drive` | rating |

**Status flow:** `candidate → shortlist → test-driven → chosen / rejected`
**Terminal:** `chosen`

## generic (no pack matched)

Minimum schema: 2–4 facts + 1–3 ratings agreed in conversation.
**Status flow:** `candidate → shortlist → chosen / rejected` · **Terminal:** `chosen`
