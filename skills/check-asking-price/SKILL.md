---
name: check-asking-price
description: Judge a Dubai apartment's or villa's asking price against registered sales of similar units in the same building or villa community — fair range, verdict, confidence, total cash to buy, rent and net return, and what to confirm — using the Dubai Wealth Index tools. Use when the user has a listing, an offer or a price for a unit in a named Dubai building or a villa or townhouse in a named community and asks "is this a good price", "is it overpriced", "what should I offer", "what is it worth" or "is this a good deal", or pastes a property listing.
---

# Check a Dubai asking price

Explicit user instructions take priority over this workflow.

## 1. Pin the unit down

You need the building (or, for a villa or townhouse, the community), the
bedroom type, the size and the asking price.

- From a pasted listing, take the building name, bedrooms, built-up area and
  price. Convert sq m to sq ft (× 10.7639). Ignore "price per sq ft" claims in
  the listing; the tool computes its own. For a villa use the built-up area,
  not the plot, and note the phase or sub-community ("Palmera 2") if given.
- Missing size: run the check without it (the tool prices the building's
  typical unit and says so), then tell the user the answer moves with size.
- Missing price: run it without one for the fair range alone.
- Ready or off-plan: pass `status` only when the user says. Otherwise the
  tool reads the project register (for a villa, which side most recent sales
  were on).
- Service charge invoice, current tenancy rent, a direct developer purchase
  (`includeAgencyFee: false`) or a letting agent (`managementPct`): pass them
  when the user gives them. Do not invent them.

## 2. Resolve the place

Call `search` with the name. A marketing name ("Vera Residences") resolves to
the registered one ("Vera Tower"); say so. If two hits are close, ask which
one. A villa community hit (kind `villa`) is checked by community.

## 3. Run the check

Apartment: call `check_property` once with `area`, `building`, `bedroom`,
`sizeSqft` and `askingPrice`. Villa or townhouse: pass `community` instead of
area and building, and `project` for the phase when the user named one; the
villa bedroom types are 1br to 6br_plus. It returns:

- `headline` and `nextStep`: the answer in one sentence each.
- `verdict.position`: `below`, `lower` (fair, lower half), `upper` (fair, upper
  half) or `above` the range, with `outsideBy` / `outsideByPct` and
  `vsEstimatePct`.
- `range`: low, middle (`estimate`) and high, in AED and per sq ft.
- `confidence.grade` and `confidence.backtest`: how often that grade's middle
  landed within 10% of real sales.
- `comparables` and `setAside`: the sales used, and the ones left out with why.
- `buying`, `rent`, `holding`, `returns`: total cash, rent, running costs,
  gross yield, net yield and net return on cash.
- `offplan`: progress, planned handover and developer, when unfinished.
- Villas: `evidence.scope` (`project` when the named phase had enough sales,
  else `all`), `evidence.matchedProjects`, and `projects`, the phase names a
  `project` can match. If the phase was too thin, say the range is
  community-wide.
- `toConfirm` and `askTheSeller`: what the record cannot see.

If `range` is null there were fewer than three comparable sales. Say so, give
the published median from `get_building` (a villa: `get_villa_community`), and
do not produce a verdict.

## 4. Answer

Lead with the verdict and the numbers behind it:

> AED 2.35M is about AED 60k (2.6%) above recent sales of similar 2-beds in
> Executive Tower G. Seven sales since February put the fair range at
> AED 2.18M–2.29M, middle AED 2.25M (high confidence: in backtests a
> high-confidence middle landed within 10% of the real price 76% of the time).

Then, briefly:

1. What to offer: `nextStep`, in your words.
2. The money: total cash to buy, rent, net return on cash. If
   `returns.netYieldPct` is null, say no service charge is on file and give
   the gross yield only.
3. Two or three comparables (date, size, price), and the set-aside count.
4. For off-plan: built %, planned handover, months to go or overdue, and the
   payment-position questions.
5. The `toConfirm` items marked `missing`, and `askTheSeller`.
6. The `canonicalUrl` for the full check.

Never call a gross yield net. Floor, view and condition (for a villa: plot,
position and upgrades) are not in registered sales: an above-range price can be
justified by them, so the answer names what would justify it rather than
calling the listing wrong.

If the user wants to view, offer or talk to an agent, offer `request_info`.
