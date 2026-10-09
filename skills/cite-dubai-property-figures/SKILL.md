---
name: cite-dubai-property-figures
description: Answer a question about what a Dubai apartment, building, area or villa community sells for, rents for or yields, using the Dubai Wealth Index tools, and quote each figure with its basis. Use when the user names a Dubai building, area or community, asks for gross rental yields, price growth or rankings, or wants to check a claim about Dubai property returns.
---

# Answer and cite a Dubai property figure

Explicit user instructions take priority over this workflow. If the user asks
for a different format or a narrower answer, follow the user.

## 1. Resolve the place

- If the user names a place, call `search` first. Use the hit's `kind` to pick
  the next tool: apartment buildings go to `get_building`, apartment areas to
  `get_area`, villa communities to `get_villa_community`.
- A `matchedName` identifies the alias a candidate matched, not proof of
  identity. Check `resolution`, score and locality first. For a confirmed exact
  match, tell the user which registered building it is before quoting: "Vera Residences is registered as Vera Tower".
- If `search` returns several plausible hits, or a tool's `resolved` block
  reports `confidence: "fuzzy"`, ask which one the user means rather than
  guessing.
- If nothing matches, say no matching published record was found. Try the
  rent purpose when relevant. Absence does not establish nonexistence or
  insufficient evidence; say below-gate only when the returned reason says so.

## 2. Pick the tool for the question

| Question | Tool |
| --- | --- |
| One building, one unit size | `get_building` with `bedroom` |
| One building, every unit size (a report) | `get_building` with `detail: "report"` |
| Every building in an area | `get_area` |
| A villa community | `get_villa_community` |
| Offices or shops (commercial, retail), one area or the best areas | `get_commercial` (`area` for one; `sort` to rank) |
| One off-plan or development project ("is it on track?", launch price, developer) | `get_project` (name is enough; area optional) |
| Off-plan market: best area, launch-to-resale uplift, live projects, late handovers | `get_offplan` (`area`, `view: "handovers"`) |
| A developer's delivery record, or the best developers | `get_developer` |
| Is the market rising, how far from the peak | `get_price_index` |
| "Which buildings…" against thresholds | `screen_buildings` |
| Two to four buildings side by side | `compare_buildings` |
| A published top list | `list_rankings`, then `get_ranking` |
| Short term, Airbnb, holiday home, short vs long let (area or city) | `get_ranking` `best-short-term-premium` with `area` — see the short-term-rental skill |
| Whether a median rests on enough sales | `get_building_transactions` |
| Citywide picture, choosing where to drill in | `get_city_overview` |
| How a figure is defined | `get_methodology` |
| The user's saved buildings | `list_favorites` (`since` for what changed) |
| What changed since an earlier edition | `whats_new` with `since` |
| Pages no typed tool covers (off-plan projects, developers) | `search`, then `fetch` |
| Any dubaiwealthindex.com URL a response links to | `fetch` — not a web browser |

Ask a follow-up only when a required input is missing, for example the bedroom
type when the user asks for "the yield" of a building that publishes several.

## 3. Quote every figure with its basis

For each number you report, include:

- the bedroom type,
- the unit (AED, AED per sqft, AED per year, percent),
- the period (usually the last 12 months),
- the data date (`dataAsOf`),
- the evidence count (number of sales or rent contracts).

Then apply these rules:

- Yields are **gross**, before service charge, agency fees, maintenance and
  vacant months. Never call one a net return.
- Rents are registered contracts, not asking rents. Prices are recorded
  transactions, not valuations.
- Short-term rental figures rest on an occupancy **assumption**. State the
  assumption returned by the tool whenever you quote a short-term figure.
- A difference between two yields is in percentage points, not percent.
- Quote a growth figure with both windows it compares — the current and prior
  medians and sample counts are in the tool's `growth` block.
- A ranking lists outliers. Never present it as the whole market.
- A `null` figure is unavailable, not zero. Use the returned reason; it may
  be suppressed, unsupported at this grain, or missing from the source.
- State whether rent evidence belongs to the building or is pooled. Pooled
  contracts are not contracts observed in that building. An empty transaction
  list cannot disprove a pooled rent figure.
- Service-charge category units and `perSqftTotal` are interpreted by the
  index under ADR-0004; the source export has no unit column. Preserve returned
  units and exclusions. A name match is not building applicability. Use the
  building record's `afterServiceCharge`, which deducts only service charge
  and must not be called fully net yield.

## 4. Stop at the edge of the data

Decline, and say why, when the user asks for live listings, viewings, agents,
anywhere outside Dubai, a figure for an individual villa, or investment,
mortgage, tax or legal advice. Offer the closest registered figure instead when
one exists.

Never invent, round away or fill in a figure the tools did not return. Link the
record's canonical URL so the user can check it.
