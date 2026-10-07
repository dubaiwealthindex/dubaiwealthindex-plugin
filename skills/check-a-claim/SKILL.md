---
name: check-a-claim
description: Test a claim about Dubai property returns — an agent's or developer's yield, price, rent or growth figure — against registered Dubai Land Department and Ejari records, using the Dubai Wealth Index tools. Use when the user quotes a figure for a Dubai building, area or community and asks whether it is true, realistic, typical or supported.
---

# Check a Dubai property claim

Explicit user instructions take priority over this workflow.

1. **Pin the claim down.** Which building or area, which unit size, which
   figure (gross or net yield, asking or achieved rent, launch or resale
   price), and over what period. If a detail is missing, ask for it or state
   the assumption you made.
2. **Find the registered figure.** `search`, then `get_building` (with the
   bedroom type) or `get_area` / `get_villa_community`. Confirm the building
   identity if the search matched another name.
3. **Compare like with like.**
   - Our yields are GROSS: registered new-contract rent over the median
     registered price in the same 12 months. Net is below gross only for the
     same income, price denominator and period with positive costs. A net yield
     on another purchase price is not directly comparable to the index gross
     yield. Identify the claim's actual basis; do not guess it.
   - A claimed short-term (Airbnb) return rests on an occupancy assumption;
     state ours from the tool result.
   - Prices are achieved, not asking.
4. **Show the evidence.** Quote the sample sizes. If the claim is about a small
   building or a thin grain, call `get_building_transactions` and describe the
   spread of actual deals.
   Preserve building versus pooled rent evidence and the returned data date.
   Link each canonical source. Budget units follow the index's ADR-0004
   interpretation. Use the building record's matched `afterServiceCharge`
   adjustment, which still excludes other operating and finance costs.
5. **Verdict.** Say whether registered records support, contradict or cannot
   test the claim, and by how much (percentage points for yields). Do not
   recommend buying or not buying.
