---
name: track-saved-buildings
description: Review the Dubai buildings saved on the user's Dubai Wealth Index account and what changed in them since an earlier data edition, using the Dubai Wealth Index tools. Use when the user asks about their saved, favourite or watched buildings, their shortlist, or what moved in the index since they last looked.
---

# Track saved buildings

Explicit user instructions take priority over this workflow.

1. Call `list_favorites`. It lists saved buildings with current figures and
   names any saved building below the publication gate in `belowGate` — report
   those as "no current figure (too few registered transactions this
   edition)", never as deleted.
2. For "what changed": call `whats_new` to see the editions, then
   `list_favorites` with `since` set to the `snapshotDate` of the edition the user means (the
   previous one if unspecified). Each change gives both editions' median price,
   rent, gross yield and counts, with the move in percent (prices, rents) and
   percentage points (yield). Changes can reflect moving windows, late registrations or corrections;
   do not infer the cause from the edition list. Distinguish `snapshotDate`
   (edition identity) from `dataAsOf` (source cutoff), which can differ.
3. For buildings not on the account, `whats_new` with `since` and `buildings`
   gives the same comparison.
   If no buildings are saved, say so and link https://dubaiwealthindex.com/account. Never
   invent a previous edition or call an unavailable figure zero. Cite record
   URLs with the bedroom type, evidence counts and both dates.
4. The tools are read-only: to add or remove a saved building, point the user
   to dubaiwealthindex.com. If `list_favorites` reports a missing scope, ask
   the user to reconnect and allow saved buildings.
