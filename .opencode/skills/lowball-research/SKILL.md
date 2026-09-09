---
name: lowball-research
description: Research an individual marketplace Listing (deal-check) or a stated buying goal (recommend) into an evidence-backed answer about whether a purchase is a good deal. Produces a Verdict (Good deal / Fair / Overpriced / Too cheap - beware) against a Fair price band built fresh from Comparables. Use when the Buyer asks "is this a good deal?", pastes a Listing URL or listing text, or states a buying goal with budget, base suburb, and travel budget. Gumtree Australia, day one. Research fresh each run, no cache.
---

# Lowball research

Turn a Listing, or a buying goal, into a researched, evidence-backed answer about whether a purchase is a good deal.



Two modes:

- **Deal-check** - one Listing. Is this a good deal?
- **Recommend** - a buying goal. Which 1-2 Listings best serve it?

Both run fresh every time. Prices, Comparables, and the New price are re-researched each run. No cache. A market moves daily. A Verdict is always evidence-backed. No Good deal or Overpriced without the band, the New price, and at least one Comparable visible to the Buyer.



## Deal-check

**Input**: a Listing URL, or pasted Listing text, or both.



1. **Read the Listing.** URL given. Read it through the fetch route below. Memo price, suburb, condition, and listed-age off the snippets. If snippets are too thin (no price or condition), ask the Buyer to paste the listing text. Paste is a first-class input, as good as a fetch. Comparables are still researched fresh either way. Pasted text. Use it as-is. Treat it as the Listing.



Completion. Ask price, item (brand/model), ,Condition (New / As-new / Good / Worn), ,suburb. Each read, or named openly unknown.



2. **Settle the New price.** Fresh retail in AU today for the exact item. From a live Aussie retailer listing. It anchors the band's ceiling. Completion. A New price with source, stated as researched this run.



3. **Collect Comparables.** Fetch fresh used Listings of the same model, .g. `site:gumtree.com.au yamaha pacifica 112v`. For each. Ask price, Condition, suburb. Label it commercial (pawn-shop, reseller, warranty, retail overhead)) or private. Commercials are flagged and never used in the band. Completion. Every Comparable named commercial-or-private, and at least one kept that is private.



4. **Widen when thin.** Fewer than 3 same-model Comparables? Search the same item class used, in or near town, .g. widen to acoustic guitars. Name the widening in the Verdict: "band built from acoustic guitars under $300, the same-model market is thin". Lower confidence hints at that. Completion. A band buildable from at least one private Comparable, and any widening named out loud.



5. **Build the Fair price band.** Take the private Comparables' asking prices. **20th-80th percentile** is the band. Cap the top at the New price. Shift by Condition, see the table below. Present as a window, "fair: $90-$150". Never a single average. Completion. A price window, anchored at or below the New price, and shifted by Condition.



6. **Position and Verdict.** Place the asking price against the band, and pick exactly one.





- inside the band -> Fair
- below the band, gap explainable by Condition or age -> Good deal
- deep below, about half or less of the band's low edge, or a gap that Condition or age cannot explain -> Too cheap - beware
- above the band -> Overpriced



Too cheap - beware doubles as the first scam screen. Check Red flags below before it lands. Completion. One Verdict, chosen by the numbers.



7. **Report to the Buyer.** Plain language. Verdict, band, New price (with source], ,at least one Comparable visible, each named private or commercial, any Red flags out loud, any widening named. Completion. The Buyer sees exactly why the Verdict is what it is.



## Recommend

**Input**: item (and key specs), max budget, base suburb, Travel budget, in km.



1. **Geocode the base suburb.** Nominatim -> coordinates. Completion. Coordinates resolved, else ask the Buyer for a nearby suburb the geocoder recognises.



2. **Search fresh for candidates.** Fetch used Listings of the item on Gumtree. For each. Ask price, suburb, Condition, listed-age. Snap each suburb to coordinates, Nominatim, and compute **haversine km** from base. Completion. Candidate Listings, each with price, condition, and km.



3. **Hard filters.** Drop everything beyond Travel budget, km, or beyond max budget. Flag commercial Listings. Exclude from recommendations. Completion. Every remaining candidate sits inside both bounds.



4. **Pick 1-2.** Rank for fit. Price against the Fair band, then km. Prefer private. Prefer distinct items or sellers. Completion.  1-2 picks, chosen for best fit, within bounds.



5. **Verdict each pick.** For each pick, run the Deal-check band procedure above, fresh Comparables, New price, band, verdict. Completion. Each pick carries: a Verdict, Fair band, New price, at least one Comparable, km, and link. All researched this run.



## Fetch routes

Gumtree Australia, day one, reached **through the search index**. Never by direct fetch (Gumtree's bot wall returns 403 to unauthenticated fetchers). The Marketplace reads off the URL, or the Buyer's statement, as data. Never as a transport branch. The same transport serves any indexable marketplace, and pasted text covers the rest.



Primary - **DuckDuckGo HTML endpoint**, AU-pinned:
`https://html.duckduckgo.com/html/?q=<query>&kl=au-en`

Fallback - **Bing RSS**:
`https://www.bing.com/search?q=<query>&format=rss&cc=au`

`site:` targets the Marketplace:
`site:gumtree.com.au yamaha pacifica 112v`



Geocode - **Nominatim** from OpenStreetMap:
`https://nominatim.openstreetmap.org/search?q=<suburb+au>&format=json&countrycodes=au`
Send a descriptive User-Agent. Nominatim policy. Distances via haversine km.



## Condition table

Condition decides where the band sits relative to the New price:

- **New / As-new**. Band sits toward the New price, capped there.
- **Good**. Mid-market used. The band as computed.
- **Worn**. Band shifted down, expect well below the New price.



## Red flags

Scam-ward signals. Never standalone Verdicts. Always spoken out loud to the Buyer:

- Holding fee or deposit before viewing.

- Insisting on non-refundable transfers.

- Shipping or interstate only, "item in one city, you send money".
- "new in box" at a fraction of the band.

- Doctored invoice, overpayment, "refund me the difference".
- Pressure to pay now or before viewing.



A deep too-cheap gap is itself a Red flag when it lands at half or less of the band's low edge.

