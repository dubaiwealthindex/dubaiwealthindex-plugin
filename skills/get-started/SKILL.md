---
name: get-started
description: Set up Dubai Wealth Index after installation, check its connection when requested, and help the user try a first property question. Use for get started, onboarding, setup, connection checks, or questions about its data and capabilities.
---

# Get started with Dubai Wealth Index

Explain the plugin in a few sentences, then offer one concrete question to try.
Explicit user instructions take priority over this guidance.

## Choose the requested path

- For "set up", "get started", or "is it connected", call `whoami` once if
  available. Report only the returned scopes, data date and saved count.
  `mcp:read` confirms property-data access; `mcp:favorites` confirms saved-building
  access. A missing favorites scope does not prevent public property research.
  A null saved count is unavailable, not zero. Do not list saved buildings
  unless the user asks to see them.
- If tools are unavailable or authentication fails, explain that the connection
  is not verified and direct the user to connect or reconnect in the host.
  Do not claim the plugin is ready merely because this skill is present.
- For "what can it do", explain capabilities without an account lookup.
- If the user already supplied a property question, proceed with `search` and
  the relevant research tool; do not stop at suggesting that same question.
- Keep setup to a brief status and one useful first question. Tool availability
  and packaged skill availability are separate: successful tools do not prove
  the host loaded a skill, and a missing skill does not prove the server is down.

## What to say

- Dubai Wealth Index publishes **registered** Dubai property figures: median
  sale prices from Dubai Land Department transactions and median rents from
  Ejari rental contracts. They are recorded prices, not asking prices or
  valuations.
- Apartments are published per building and per area. Villas and townhouses on
  plot titles are published per community, because the register records no
  building for a villa.
- Every figure carries its bedroom type, unit, period, data date and the number
  of transactions behind it.
- All tools are read-only. Signing in also lets it read the buildings the user
  has saved on dubaiwealthindex.com.

## What it can answer

- One building or villa community: price, rent, gross yield, growth — or a
  full report on a building covering every unit size.
- An area: every published building, ordered by yield, price or growth.
- A screen across the city: buildings above a yield floor or inside a price band.
- A side-by-side comparison of two to four buildings on the same bedroom type.
- A published ranking, or the individual transactions behind a median.
- Offices and shops: achieved prices, rents, yields, liquidity and leasing
  demand by area.
- Off-plan: any registered project's progress, planned and recorded dates,
  launch prices and developer; which areas' launches resold highest; late
  handovers; developers ranked on delivery; the citywide price index.
- Short-term rentals: which buildings in an area out-earn the registered lease
  on Airbnb, the break-even occupancy, and short- versus long-let yield.
- Whether an agent's or developer's claimed yield holds up against registered
  records.
- The user's saved buildings, and what changed in them since an earlier edition.

- Registered apartment sales activity: counts, transaction values, price ranges
  and evidenced year-over-year change by area and bedroom.
- Pending project supply: planned dates, progress and known units, with source
  dates, overdue projects and missing-data coverage kept explicit.

## What it cannot do

Say so plainly if asked: it holds no listings, cannot book viewings or contact
agents, covers Dubai only, and does not give investment, mortgage, tax or legal
advice. Yields are gross, and short-term rental figures are modelled from a
stated occupancy assumption.

## Suggested first questions

Offer one of these, adapted to what the user mentioned:

- "What does a 1-bedroom in Vera Tower, Business Bay rent for and yield?"
- "Which Dubai Marina buildings have the highest gross rental yields?"
- "Is a 9% yield in Vera Tower realistic?"
- "What is best for short-term rental in Business Bay?"
- "Compare 3-bedroom villa rents in Arabian Ranches and The Springs."

Do not quote a figure from memory; figures come only from tool results.

Residential service-charge budgets are available through `get_service_charge_budgets`.
Preserve the property-group identity, budget year, source date and returned units.
Unit labels and `perSqftTotal` follow the index's ADR-0004 interpretation of source
categories; the source export itself has no unit column. A name match is not a
verified building match. Use the building record's `afterServiceCharge` for its
matched charge; this deducts service charge only and is not a fully net return.
