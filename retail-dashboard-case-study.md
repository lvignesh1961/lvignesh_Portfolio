---
title: Retail Sales & Forecast Dashboard — [Your Name]
meta-description: A formula-driven Excel dashboard that turns a raw order export into a live sales dashboard and a segment-level demand forecast through FY2030 — no add-ins, no macros required.
meta-keywords: Excel dashboard, retail analytics, SUMIFS, INDEX MATCH, FORECAST function, demand planning, sales forecasting
---

# Retail Sales & Forecast Dashboard

*Excel · Formula-driven analytics · Demand planning*

I build a lot of one-off analysis for stakeholders who will never open a script, a notebook, or a
BI tool — but who live in Excel all day. This is one of those: a single workbook that takes a raw
retail order export and turns it into two boardroom-ready dashboards, one for what already
happened and one for what's likely to happen through FY2030. No Power BI, no Python, no plugins —
just formulas, and it still recalculates live the moment new orders are pasted in.

[Try the interactive tour](#) · [Download the workbook](#)

*A seven-step walkthrough of both dashboards — category and segment breakdowns, top accounts,
regional splits, and the FY2030 forecast — built from the same numbers as the live workbook.*

## Why I built this

Most retail teams I've worked with don't lack data — they lack a place to look at it that doesn't
require asking someone else to run a query. The brief I set myself was: given a flat export of
orders (customer, product, region, date, revenue — the shape almost every retailer already has),
build something a category manager or ops lead can open cold and immediately answer three
questions: what's selling, who's buying it, and where the business is headed if nothing changes.

The two dashboards split those questions cleanly. The **Sales dashboard** answers the first
two against the trailing four years of actuals — category mix, segment mix, top customers and
orders, regional performance. The **Forecast dashboard** answers the third, projecting each
customer segment out to FY2030 and flagging which one deserves more inventory and sales attention.

## What this is — and what it isn't

This isn't a machine-learning forecast. Every number on both dashboards comes from three
built-in Excel mechanisms: `SUMIFS` for the aggregations, `INDEX`/`MATCH` for the
lookups and rankings, and the native `FORECAST` function — linear regression against
the historical years — for the FY2018–30 projections. Nothing here needs a data connection,
a refresh button, or software the recipient doesn't already have.

What it demonstrates instead is **structuring a workbook so formulas can do the job a
script normally would** — a hidden calculation layer feeding two clean, presentation-ready
sheets, built to survive a stakeholder pasting in a new month of orders without anything breaking.

## The architecture

The workbook is deliberately split into three layers, and only the top one is ever meant to be
looked at:

```
Retails Order Full Dataset  (5,009 raw orders, one row each)
            │
            ▼
Calc_* sheets  (Calc_Category, Calc_Segment, Calc_Region, Calc_State,
                Calc_Customers, Calc_Orders, Calc_Top10, Calc_Recommend)
   SUMIFS / INDEX-MATCH aggregation + ranking, one sheet per question
            │
            ▼
Sales dashboard  +  Forecast Dashboard
   formatted, chart-driven, formula-linked — nothing typed by hand
```

The **Sales dashboard** reads straight from the `Calc_*` sheets: category and segment
summaries, a top-10 customers and top-10 orders ranking built with `LARGE` +
`INDEX`/`MATCH` rather than a manual sort, and a region/state breakdown that feeds a
built-in Excel map chart.

The **Forecast dashboard** takes the FY2014–17 actuals per segment, projects FY2018–30
with `FORECAST`, and layers on a small recommendation engine: each segment's FY14–17
CAGR is compared against the three-segment average, and anything growing faster than average gets
flagged **Increase orders** rather than **Maintain orders**. It's a rule, not a
model — but it's a rule a demand planner can audit in about ten seconds, which matters more than
sophistication for a document that's going to sit in someone else's inbox.

## The parts that were actually hard

**Keeping 5,009 rows fast without a single macro.** `SUMIFS` and
`INDEX`/`MATCH` are cheap individually, but the Sales dashboard alone runs dozens of them
against a live 5,009-row table. The fix was pushing every aggregation into its own
`Calc_*` sheet computed once, so the dashboard sheets only ever reference a handful of
pre-summarized cells instead of scanning the raw data repeatedly — the same reason you'd stage a
query instead of nesting ten subqueries.

**Ranking without volatile array formulas.** Top-10 customers and top-10 orders both
needed a live rank that re-sorts itself as data changes, without locking the workbook into a
version of Excel that supports dynamic arrays. `LARGE` picks the *n*th-highest revenue
value; `INDEX`/`MATCH` then finds the row that produced it. It's two older functions
doing what a modern `SORT`/`FILTER` combo would do in one — chosen specifically so
the file opens cleanly in whatever Excel version the recipient has.

**A forecast that doesn't overreact to one good year.** `FORECAST` against only four
data points is sensitive to a single unusual year skewing the trend line. The projection is run
per segment rather than on total revenue, which keeps a strong Corporate quarter from dragging the
whole company's forecast — and the recommendation logic checks the segment's own
multi-year CAGR against the group average, rather than reacting to any single year in isolation.

**Macros as an optional layer, not a dependency.** The dashboards are fully live without
any VBA — that was a hard requirement, since a recipient might not enable macros at all. Two
optional macros (`RefreshDashboard`, `ExportDashboardsToPDF`) are documented on
their own sheet as ready-to-paste code for anyone who wants a one-click recalculation or PDF
export, but the workbook never depends on them working.

## What the numbers say

Across the trailing four years the business did **$2.30M** in revenue on
**5,009** orders and **37,873** units — an average order of
**$458.61**. The forecast projects total sales growing from **$733K** in
FY2017 to **$1.86M** by FY2030, a **53%** increase, with
**Corporate** the clear outlier: it's growing at roughly **2.3×** by
FY2030 against FY2017, well ahead of Consumer and Home Office, which is what drives its
**Increase orders** flag.

## What I'd do differently at production scale

A single 5,009-row export is comfortably inside what `SUMIFS` and `FORECAST`
can chew through instantly. That stops being true somewhere in the low hundreds of thousands of
rows, where a workbook like this needs to become a Power Query or database-backed model instead of
formulas reading a live sheet. The forecast logic is also honestly simple by design — a four-point
linear regression is the right amount of rigor for a one-page dashboard, and the wrong amount for
an actual inventory-planning decision, which deserves seasonality, promotional effects, and a wider
history than four years. The demo is the shape of the analysis; a real deployment is the modeling
work underneath it.

## Stack

Microsoft Excel — `SUMIFS`, `INDEX`/`MATCH`, `LARGE`, `FORECAST`,
native chart and map objects. Optional VBA macros for one-click refresh and PDF export.

[Try the interactive tour](#) · [Download the workbook](#)
