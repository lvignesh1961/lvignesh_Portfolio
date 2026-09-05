---
title: Inventory Management Dashboard — [Your Name]
meta-description: A live reorder-point and ABC-analysis dashboard for a 1,000-SKU, 5-warehouse inventory — no macros, filterable by category, warehouse, and supplier, formula-driven end to end.
meta-keywords: Excel dashboard, inventory management, reorder point, ABC analysis, Pareto analysis, SUMIFS, supply chain
---

# Inventory Management Dashboard

*Excel · Inventory analytics · Reorder planning*

A retailer with a few hundred SKUs across a couple of warehouses can usually eyeball what's
running low. Past a thousand SKUs and five warehouses, that stops being true — and the person who
finds out first is usually a customer standing in front of an empty shelf. This workbook was my
attempt to build the thing that catches it earlier: a live reorder-point and stock-health dashboard
over a 1,000-SKU, 10-category, 5-warehouse e-grocery inventory, filterable by category, warehouse,
and supplier, with nothing running behind the scenes but formulas.

[Try the interactive tour](#) · [Download the workbook](#)

*A seven-step walkthrough — the shared filter cells, the 20 most urgent reorders, category-level
reorder recommendations, stock health, and the ABC classification — built from the same numbers as
the live workbook.*

## Why I built this

Two questions dominate inventory management and they're in tension with each other: what needs to
be reordered right now, and what's already sitting on the shelf that shouldn't be. Most templates I
found online answer one or the other. This workbook answers both, on two dedicated sheets that
share the same filter mechanism, so a category manager and a warehouse manager can both open the
same file and get the view they actually need.

The **Order Management dashboard** answers the reorder question: which SKUs are below their
reorder point, how much to order, and which suppliers and categories need the most attention this
week. The **Inventory dashboard** answers the health question: how much value is sitting in
the warehouse, how long it's been there, and which items deserve tight control versus which can be
reviewed quarterly.

## What this is — and what it isn't

This isn't a demand-forecasting model — it doesn't fit a distribution or run a Monte Carlo
simulation. It's a **rules engine built entirely from Excel's built-in functions**:
`SUMIFS` for every filtered rollup, `LARGE` + `INDEX`/`MATCH` for the
urgency ranking, and a straightforward reorder-quantity formula. What it demonstrates is
structuring those rules so a warehouse manager gets a defensible, auditable answer — not a black
box — every time they open the file.

## The architecture

Three shared criteria cells (`CritCat`, `CritWH`, `CritSup`) sit behind
both dashboards. Every dashboard formula references those three cells instead of a hard-coded
value, so setting Category, Warehouse, or Supplier once re-filters everything on the sheet:

```
Inventory Data (1,000 SKUs, one row each: category, warehouse, supplier,
                on-hand qty, reorder point, forecast, days-in-stock)
            │
   CritCat / CritWH / CritSup  (three shared filter cells)
            │
            ▼
Order Management dashboard          Inventory dashboard
  reorder-point shortfall             stock value & age
  ranked by urgency (LARGE +          category summary
  INDEX/MATCH)                        ABC classification (Pareto)
  category-level reorder rollup       top-20 Pareto chart
```

The reorder-point logic is one formula, applied per SKU:
`MAX(Forecast_Next_30d − Qty_On_Hand + Safety_Stock, 0)` — the quantity needed to cover
the next 30 days of expected demand plus a buffer, floored at zero so a well-stocked SKU never
shows a negative reorder amount.

The ABC classification sorts all 1,000 SKUs by value, then buckets them once cumulative value
share crosses 80% (Class A) and 95% (Class B), with everything else falling to Class C — the
standard Pareto rule for deciding where inventory-control effort actually pays off.

## The parts that were actually hard

**A ranking that survives three filters at once.** The top-20-most-urgent list needed
to keep working whether Category, Warehouse, and Supplier were all set to "All" or narrowed to one
of each. `LARGE` against a `SUMIFS`-scoped array, paired with
`INDEX`/`MATCH` to pull back the SKU details for whichever row produced that value,
handles the three-way filter without a single volatile array formula or a macro to resort a table.

**Keeping the reorder quantity from going negative or absurd.** A raw shortfall formula
would show a negative number for a well-stocked SKU, or a wildly high number for one with a data
error in its forecast. Flooring at zero with `MAX` handles the first; the second was solved
by sanity-bounding the forecast input itself rather than trying to catch every downstream formula
that touched it.

**Making the ABC cutoffs move with the filtered data, not the whole dataset.** An ABC
analysis is only useful if Class A actually represents the top ~80% of *whatever's currently
filtered* — not always the top 80% of the full 1,000 SKUs. The cumulative-percentage column
recalculates against the filtered set every time, so narrowing to one warehouse gives that
warehouse its own honest A/B/C split instead of inheriting the company-wide one.

**Two "zero" numbers that mean different things.** Out-of-stock SKUs (2) and dead-stock
SKUs (0) both show up as small numbers near the top of the dashboard, and it would be easy to read
them as good news together. They're opposite failure modes — one is too little inventory, the
other is too much sitting unsold past 180 days — and the dashboard keeps them as separate KPIs
specifically so neither gets glossed over.

## What the numbers say

Across 1,000 SKUs, **261** currently sit below their reorder point, worth
**$1,763,716** to bring back to target. Supplier on-time delivery averages
**84.7%** with a **4.03**-day average lead time, and only
**2** SKUs are fully out of stock. On the health side, **$1,259,531** is
tied up on the shelf right now — Class A alone (200 SKUs, the top 20%) accounts for
**53.5%** of that value, which is exactly the population that should get the tightest
cycle-count and safety-stock discipline.

## What I'd do differently at production scale

A thousand SKUs across five warehouses is still small enough for `SUMIFS` to recalculate
instantly. Past tens of thousands of SKUs or dozens of warehouses, the honest version moves this
logic into Power Query or a proper database view, both for calculation speed and because the
reorder formula itself deserves more nuance at scale — seasonality by category, supplier-specific
lead-time variance, and safety stock sized to service-level targets rather than a flat buffer. The
ABC thresholds are also a simplification: a real deployment would let 80/95 move per category
rather than applying one cutoff company-wide, since a high-turnover category like Beverages and a
slow one like Household don't behave the same way.

## Stack

Microsoft Excel — `SUMIFS`, `INDEX`/`MATCH`, `LARGE`, native
Pareto/combo charts, data-validation dropdown filters.

[Try the interactive tour](#) · [Download the workbook](#)
