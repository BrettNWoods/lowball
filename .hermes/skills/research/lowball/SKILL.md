---
name: lowball
description: "Marketplace deal-check and recommendation — research a listing URL or stated buying goal into an evidence-backed verdict (Good deal / Fair / Overpriced / Too cheap - beware) using fresh comparables from Gumtree Australia. No cache."
version: 1.0.0
author: BrettNWoods (ported to Hermes Agent)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [marketplace, research, gumtree, shopping, deal-check]
    related_skills: [web_search]
---

# Lowball — Marketplace Deal Research

`NO TIME WASTERS, I KNOW WHAT I HAVE`

Research an individual marketplace Listing (deal-check) or a stated buying goal (recommend) into an evidence-backed answer about whether a purchase is a good deal.

## Two Modes

- **Deal-check** — one Listing. Is this a good deal?
- **Recommend** — a buying goal. Which 1-2 Listings best serve it?

Both run fresh every time. Prices, Comparables, and the New price are re-researched each run. No cache. A market moves daily.

**Verdict options (exactly one, chosen by the numbers):**
- Good deal
- Fair
- Overpriced
- Too cheap - beware

## When to Use

- Buyer asks "is this a good deal?" or pastes a Gumtree URL / listing text
- Buyer states a buying goal with budget, base suburb, and travel budget (km)
- Any secondhand marketplace deal evaluation

**Target:** Gumtree Australia, day one. Pasted listing text is a first-class input.

## Deal-check

**Input:** a Listing URL, or pasted Listing text, or both.

### Step 1: Read the Listing

If URL given, search for it via DuckDuckGo HTML endpoint (`site:gumtree.com.au` scoped). Memo: price, suburb, condition, listed-age. If snippets are too thin (no price or condition), ask the Buyer to paste the listing text. Pasted text is used as-is — treat it as the Listing.

Completion: Ask price, item (brand/model), Condition (New / As-new / Good / Worn), suburb. Each read, or named openly unknown.

### Step 2: Settle the New price

Fresh retail in AU today for the exact item. Research via DuckDuckGo HTML (AU-pinned). Find a live Aussie retailer listing. It anchors the band's ceiling.

Completion: A New price with source, stated as researched this run.

### Step 3: Collect Comparables

Fetch fresh used Listings of the same model using `site:gumtree.com.au <model>` searches via DuckDuckGo HTML endpoint (AU-pinned).

For each Comparable: Ask price, Condition, suburb. Label it commercial (pawn-shop, reseller, warranty, retail overhead) or private. Commercials are flagged and never used in the band.

Completion: Every Comparable named commercial-or-private, and at least one kept that is private.

### Step 4: Widen when thin

Fewer than 3 same-model Comparables? Search the same item class used, in or near town, e.g. widen from "yamaha pacifica 112v" to "acoustic guitars under $300". Name the widening in the Verdict: "band built from acoustic guitars under $300, the same-model market is thin". Lower confidence hints at that.

Completion: A band buildable from at least one private Comparable, and any widening named out loud.

### Step 5: Build the Fair price band

Take the private Comparables' asking prices. **20th-80th percentile** is the band. Cap the top at the New price. Shift by Condition (see Condition table below). Present as a window, "fair: $90-$150". Never a single average.

Completion: A price window, anchored at or below the New price, and shifted by Condition.

### Step 6: Position and Verdict

Place the asking price against the band and pick exactly one:

- inside the band → **Fair**
- below the band, gap explainable by Condition or age → **Good deal**
- deep below, about half or less of the band's low edge, or a gap that Condition or age cannot explain → **Too cheap - beware**
- above the band → **Overpriced**

Too cheap - beware doubles as the first scam screen. Check Red flags below before it lands.

Completion: One Verdict, chosen by the numbers.

### Step 7: Report to the Buyer

Plain language. Verdict, band, New price (with source), at least one Comparable visible (each named private or commercial), any Red flags out loud, any widening named.

Completion: The Buyer sees exactly why the Verdict is what it is.

## Recommend

**Input:** item (and key specs), max budget, base suburb, travel budget (km).

### Step 1: Geocode the base suburb

Use Nominatim from OpenStreetMap: `https://nominatim.openstreetmap.org/search?q=<suburb+au>&format=json&countrycodes=au`. Send a descriptive User-Agent (Nominatim policy).

Completion: Coordinates resolved, else ask the Buyer for a nearby suburb the geocoder recognises.

### Step 2: Search fresh for candidates

Fetch used Listings of the item on Gumtree via `site:gumtree.com.au <item>` on DuckDuckGo HTML (AU-pinned). For each: Ask price, suburb, Condition, listed-age. Snap each suburb to coordinates (Nominatim) and compute haversine km from base.

Completion: Candidate Listings, each with price, condition, and km.

### Step 3: Hard filters

Drop everything beyond travel budget (km) or beyond max budget. Flag commercial Listings. Exclude from recommendations.

Completion: Every remaining candidate sits inside both bounds.

### Step 4: Pick 1-2

Rank for fit: price against the Fair band, then km. Prefer private. Prefer distinct items or sellers.

Completion: 1-2 picks, chosen for best fit, within bounds.

### Step 5: Verdict each pick

For each pick, run the Deal-check band procedure (Steps 2-7 above): fresh Comparables, New price, band, verdict.

Completion: Each pick carries: a Verdict, Fair band, New price, at least one Comparable, km, and link. All researched this run.

## Condition Table

Condition decides where the band sits relative to the New price:

| Condition  | Band Position |
|------------|---------------|
| New / As-new | Band sits toward the New price, capped there |
| Good       | Mid-market used. The band as computed |
| Worn       | Band shifted down, expect well below the New price |

## Red Flags (Scam Signals)

Never standalone Verdicts. Always spoken out loud to the Buyer:

- Holding fee or deposit before viewing
- Insisting on non-refundable transfers
- Shipping or interstate only, "item in one city, you send money"
- "New in box" at a fraction of the band
- Doctored invoice, overpayment, "refund me the difference"
- Pressure to pay now or before viewing
- A deep too-cheap gap itself (half or less of the band's low edge) is a Red flag

## Fetch Routes

**Primary — DuckDuckGo HTML endpoint, AU-pinned:**
`https://html.duckdckgo.com/html/?q=<query>&kl=au-en`

**Fallback — Bing RSS:**
`https://www.bing.com/search?q=<query>&format=rss&cc=au`

Use `site:` to target the Marketplace:
`site:gumtree.com.au yamaha pacifica 112v`

**Geocode — Nominatim (OpenStreetMap):**
`https://nominatim.openstreetmap.org/search?q=<suburb+au>&format=json&countrycodes=au`
Send a descriptive User-Agent. Distances via haversine km.

## Verification

After running either mode, verify:
- [ ] New price sourced from a live Aussie retailer
- [ ] At least one private Comparable found
- [ ] 20th-80th percentile band computed correctly
- [ ] Verdict chosen by the numbers, not intuition
- [ ] Any widening or thin-market caveat named out loud
- [ ] Red flags spoken to the Buyer, if present