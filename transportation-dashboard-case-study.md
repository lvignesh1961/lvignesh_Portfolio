---
title: Transportation Analysis Dashboard — [Your Name]
meta-description: A shipping-cost-and-revenue dashboard that ties carrier, transport mode, and route performance together against a live 100-shipment export — built entirely on SUMIF and AVERAGEIF.
meta-keywords: Excel dashboard, transportation analysis, shipping cost, logistics, SUMIF, AVERAGEIF, supply chain analytics
---

# Transportation Analysis Dashboard

*Excel · Logistics analytics · Shipping cost*

Revenue and shipping cost usually live in two different reports, which makes an obvious question
surprisingly hard to answer quickly: is this carrier actually cheap, or does it just look that way
because it happens to move your highest-margin products? This dashboard puts both sides in one
sheet — revenue by product, cost by carrier, cost by transport mode and route — so the comparison
takes one glance instead of a VLOOKUP between two files.

[Try the interactive tour](#) · [Download the workbook](#)

*A six-step walkthrough — product revenue, carrier performance, mode and route cost, and the
formulas behind all of it — built from the same numbers as the live workbook.*

## Why I built this

I built this against a 100-shipment e-commerce beauty export with a specific tension in mind:
shipping cost and transportation cost aren't the same thing, and a dashboard that quietly merges
them makes it impossible to tell whether a carrier is expensive or a route is expensive. The brief
was to keep both legible on their own, while still giving a single "is shipping eating our margin"
number at the top for whoever only has ten seconds to look.

Three questions drive the layout: what's selling and at what price, which carrier is actually
worth using once cost and speed are both on the table, and which transport mode or route is
quietly the cheapest option available.

## What this is — and what it isn't

There's no optimization solver here — it doesn't recommend a carrier switch or re-route
shipments. It's **descriptive analytics done right**: `SUMIF` and
`AVERAGEIF` against a single source sheet, structured so that carrier, mode, and route can
each be compared on the same footing — shipments, cost, speed, and cost as a share of revenue —
rather than whichever metric happened to be easiest to pull.

## The architecture

Everything on the dashboard reads from one 100-row shipment sheet
(`supply_chain_data_TR`), with each dashboard section aggregating a different column of it:

```
supply_chain_data_TR  (100 shipments: product type, carrier, shipping cost,
                        shipping time, transport mode, route, transport cost)
            │
            ├──▶ SUMIF by Product Type   → revenue, units, avg price
            ├──▶ SUMIF/AVERAGEIF by Carrier → shipments, cost, speed, cost % of revenue
            └──▶ SUMIF/AVERAGEIF by Mode & Route → transport cost, avg cost/shipment
```

The one deliberate design choice worth calling out: **shipping cost** (a per-order carrier
charge, column M in the source data) and **transport cost** (a per-shipment mode/route
charge, column X) are tracked as two separate figures throughout, and only combined into a single
"total shipping cost" KPI at the very top of the dashboard — every section below that KPI keeps
them apart.

## The parts that were actually hard

**Not double-counting two different costs that both sound like "shipping."** Early
drafts of this dashboard summed the per-order carrier cost and the per-shipment transport cost
into every downstream table, which inflated the mode/route breakdown by whatever the carrier
happened to charge on that same order. The fix was structural: the combined figure only appears
once, in the header KPI, and every other table pulls from exactly one of the two source columns.

**Comparing carriers on cost per dollar of revenue, not cost per shipment.** Carrier A
and Carrier C ship similar volumes but very different products, so a raw
average-cost-per-shipment comparison would have been misleading. Cost as a percentage of the
revenue that carrier actually generated turned out to be the fairer comparison, and it's the
number that surfaces which carrier is genuinely a good deal.

**Making mode and route comparable when they're not mutually exclusive.** A shipment has
both a transport mode (Air, Rail, Road, Sea) and a route (A, B, C) — they're two different cuts of
the same 100 rows, not a hierarchy. Keeping them as two side-by-side tables, both driven by the
same `SUMIF`/`AVERAGEIF` pattern, avoided the temptation to force a route-within-mode
tree that the data doesn't actually support.

## What the numbers say

Across 100 shipments the business generated **$577,605** in revenue on
**46,099** units, at a combined shipping and transport cost of
**$53,479** — **9.3%** of revenue. Skincare drove the largest share of
revenue (**41.8%**) despite the fewest units among the three product types, on the
strength of a materially higher average price. On the logistics side, **Sea** is the
cheapest transport mode at **$417.82** per shipment against **$561.71**
for Air, and **Carrier B** offers the best combination of volume, speed, and cost as a
share of revenue among the three carriers.

## What I'd do differently at production scale

A hundred shipments is small enough that `SUMIF`/`AVERAGEIF` recalculates
instantly and a human can sanity-check every row. At real freight volume — thousands of shipments
a month across dozens of routes — this becomes a job for a proper fact table and pivot-based or
BI-tool aggregation, both for performance and because the carrier comparison deserves more nuance:
cost and speed should be weighted by shipment value and service-level requirements, not treated as
equally important for a $12 lipstick and a $200 skincare set. The mode/route split would also
benefit from a true hierarchy once volume justifies it — which routes each mode is actually used
on — rather than two independent tables.

## Stack

Microsoft Excel — `SUMIF`, `AVERAGEIF`, native combo charts, data-driven
KPI cells.

[Try the interactive tour](#) · [Download the workbook](#)
