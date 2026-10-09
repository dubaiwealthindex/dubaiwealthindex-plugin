---
name: short-term-rental
description: Answer Dubai short-term rental questions — Airbnb, holiday home, short let versus long let, rental arbitrage, which buildings or bedroom types work best for short term in an area — using the Dubai Wealth Index tools. Use when the user asks what is best or most profitable for short term in a Dubai area or building, whether to short-let or lease, or about nightly rates, occupancy or break-even.
---

# Short-term rentals in Dubai

Explicit user instructions take priority over this workflow. Stay on this
server: every short-term figure the website shows is returned by these tools,
and `fetch` reads any dubaiwealthindex.com page a response links to.

## 1. Pick the call

| Question | Call |
| --- | --- |
| "What is best for short term in <area>?" | `get_ranking` with `ranking: "best-short-term-premium"`, `area` |
| Short let vs long lease in an area (or arbitrage: rent then sublet) | same, `sort: "premium"` (the default with an area) |
| Buy to short-let: return on the purchase price | same, `sort: "yield"` |
| One bedroom type ("studios in JVC") | same, add `bedroom` |
| Where in Dubai | `get_ranking` without `area`; `list_rankings` lists the areas that publish |
| One building | `get_building` with `detail: "report"`: each bedroom's `shortLet` block |

Pass the area as the user wrote it; names and slugs both resolve. One call
answers the area question: do not follow it with `get_building` per row — each
row already carries the figures below.

## 2. Read the row

- `medianAdr` nightly rate, with `listingCount` and `rateSource`
  (`building` = sampled in that building, `vicinity` = nearby listings).
- `netRevenue` a year after platform fee, cleaning and utilities.
- `medianRent` the registered annual lease; `surplusAfterLease` the gap in AED;
  `premiumVsLease` the ratio (1.3 = 30% more than leasing).
- `breakEvenOccupancyPct` the occupancy at which short letting only matches
  the lease — the most useful risk figure: the lower, the more margin.
- `netYieldPct` beside `longLetGrossYieldPct`: the same purchase price under
  both strategies, so the gap is in percentage points.

## 3. Answer

- Lead with the direct answer: the buildings (and bedroom types) that come
  out ahead, how far ahead of the lease, and the break-even occupancy.
- If the user asked "short or long", say which wins in this model and by how
  much, then the break-even as the condition for it.
- Give `areaMedians` for context, and say the list is every qualifying
  building in the area: only rows earning more than the lease are listed, so
  a building missing from it does not beat its lease in this model.
- Before any short-term figure, state the assumptions from the response
  notes: occupancy is an assumption (not a measurement), nightly rates are
  public asking rates (not bookings), and the unit is self-managed —
  professional management typically takes 15–25% of revenue on top.
- Holiday-home letting in Dubai needs a DET permit; mention it once when the
  user is deciding whether to do it. Do not give investment advice.
- Link `canonicalUrl` and each row's `url`, with `dataAsOf`.

If the area returns `below-gate`, no building there has both a nightly-rate
sample and a registered lease beating it — say so, then offer the citywide
list or the area's long-let yields (`get_area`), as the suggestions name.
