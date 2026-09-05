# Google Site Content — Vignesh Portfolio

Paste-ready copy and layout plan for building the Google Site. Text blocks are written to be copied directly into Sites text boxes; image references point to files in [dashboard_screenshots/](dashboard_screenshots/), now public on GitHub at `https://github.com/lvignesh1961/lvignesh_Portfolio`. Sourced from the resume content already captured in this doc and the three dashboard workbooks (see [Vignesh_Portfolio.md](Vignesh_Portfolio.md) for full technical detail behind each project).

**Using the images in Google Sites**: repo is now public, so each screenshot has a direct, hotlinkable URL. In Sites, use **Insert → Image → By URL** and paste the raw URL listed under each image below (pattern: `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/<filename>` — all 9 verified resolving).

---

## Site map

```
Home
 ├─ Hero / intro
 ├─ Skills & toolset
 ├─ Experience
 ├─ Project 1 — Inventory Management Dashboard
 ├─ Project 2 — Retail Sales & Forecast Dashboard
 ├─ Project 3 — Transportation Analysis Dashboard
 └─ Contact
```

Google Sites works best as a single long Home page with named sections (using the built-in section dividers) rather than separate sub-pages, since each project is only a few paragraphs + images — but if you'd rather have 3 separate "Projects" sub-pages under a Projects nav item, the same copy blocks below work unchanged, just split at the `# Project` headings.

---

## 1. Header / navigation

**Site name (top-left, also becomes the browser tab title):**
```
Vignesh | Supply Chain & Data Analytics
```

**Navigation menu items:**
```
Home    Projects    Contact
```

---

## 2. Hero section

**Layout**: banner/header block at the top of the page. Use Sites' "Banner" section with a plain color background (navy or dark slate reads well against the dashboard screenshots' blue theme) or a full-bleed image if you add a profile photo later.

**Headline:**
```
Vignesh
```

**Subheadline:**
```
Supply Chain & Data Analytics Professional
```

**Body text:**
```
Data-driven Supply Chain Analytics professional with 6+ years of experience in demand
forecasting, inventory planning, and supply-demand analytics supporting revenue and
operational decisions across large-scale manufacturing and supply chain environments.
Skilled in SQL and Python for analyzing historical performance and developing reporting
tools that improve inventory accuracy and optimization. Experienced partnering
cross-functionally with finance, operations, and supply chain teams to drive
data-informed planning. Currently expanding expertise in Power BI and the Microsoft
Fabric ecosystem.
```

**Optional button** (only if you add a PDF resume later):
```
Button text: Download Résumé
Link: [add link to resume PDF once uploaded]
```

---

## 3. Skills & toolset section

**Section heading:**
```
Skills & Technical Toolset
```

**Body text** (works well as a plain paragraph or as a row of Sites "text chips" if you lay them out manually):
```
Power BI · SAP · Power Query · Azure · Microsoft Suite · MS Excel · Google Suite ·
Google Cloud · Jira · Confluence · Tableau · AWS Redshift · Oracle NetSuite ·
Snowflake · Python · Pandas · NumPy · PyTorch · SQL · Linux Environments · Plotly · VBA
```

---

## 4. Experience section

**Section heading:**
```
Experience
```

**Block 1:**
```
Global Supply Chain Analyst
Motorola Solutions, Vancouver, BC — September 2024 – Present

Partner with cross-functional stakeholders across supply chain, finance, and lifecycle
management teams to translate complex planning requirements into SQL (AWS Redshift)
and Python-based Tableau dashboards, improving inventory and financial decision
accuracy by 25% across portfolio planning initiatives. Gather and document reporting
requirements from product management and finance teams, translating business needs
into SQL-based ETL data pipelines and KPI/OKR dashboards that monitor lifecycle risk
and execution readiness across 80,000+ components and product dependencies. Developed
portfolio-level analytics frameworks for S&OP and demand planning to monitor Last Time
Buy timing, EOL exposure, ramp-up/ramp-down curves, and product transition readiness.
Led cross-functional analytics initiatives using Jira and Confluence, improving project
delivery timelines by 20%. Documented supply risk assessments and scenario modeling
supporting $5M+ quarterly revenue commitments.
```

**Block 2:**
```
Supply Planning Analyst
Motorola Solutions, Vancouver, BC — January 2024 – September 2024

Led development of Excel-based planning tools (VBA, Macros) to analyze global
inventory positions and demand signals, improving inventory accuracy by 15%.
Optimized SAP-based purchase order and delivery planning workflows using VBA
automation and SAP scripting, reducing procurement cycle time and manual processing
effort by 50%. Built revenue-impacting supply-demand risk models and dashboards that
supported $1M in quarterly revenue commitments. Performed MRP logic and SAP work
orders for aggregate and mix planning across capacity, supply/demand, and lead-time
constraints.
```

**Block 3:**
```
Supply Chain Planner
CIMtech Mfg. Inc., Surrey, BC — August 2021 – January 2024

Developed cost-effective production plans yielding 25% savings in setup and tooling
costs. Improved supplier OTIF (On-Time In-Full) from 60% to 68%. Built ERP-based
dashboards improving visibility into production performance and planning metrics.
Ran capacity planning using demand/forecast parameters, takt time, and OEE. Served as
analytics liaison between operations and executive leadership.
```

---

## 5. Project sections

Each project section follows the same pattern: heading → one-paragraph summary → key metrics line → screenshot(s) → short "what to notice" caption. Insert images in the order listed — Sites lets you drop multiple images into one section as a row or stack.

### Project 1 — Inventory Management Dashboard

**Heading:**
```
Inventory Management Dashboard
```

**Summary:**
```
An Excel-based inventory analytics workbook for an e-grocery operation, covering
1,000 SKUs across 10 categories. Combines an Order Management view (reorder
prioritization) with an Inventory Dashboard view (stock health and ABC/Pareto
analysis) — built entirely on live SUMIF/AVERAGEIF-style formulas against a raw
SKU-level dataset, no pivot tables.
```

**Key metrics line** (pulled straight from the dashboard KPI cards):
```
1,000 SKUs tracked  ·  $1.26M total inventory value  ·  261 SKUs needing reorder
84.7% avg. supplier on-time  ·  10.0 avg. days of inventory  ·  524 slow-moving SKUs
```

**Images (in order):**
1. Reorder KPIs + Top 20 most urgent SKUs + reorder recommendation by category
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Order_Management_Dashboard_1.png`
2. Inventory KPIs + product summary by category + ABC class breakdown
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Inventory_Management_Dashboard_1.png`
3. ABC Pareto detail + Top 20 SKUs by value
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Order_Management_Dashboard_2.png`

**Caption / "what to notice":**
```
The reorder logic is fully rules-based: Suggested Qty = MAX(Forecast_Next_30d − Qty On
Hand + Safety Stock, 0), ranked by shortfall against each SKU's reorder point. The ABC
analysis follows the classic 80/20 rule — Class A SKUs (53.5% of value from just 200
SKUs) get the tightest control, while Class C gets simple min-max review.
```

---

### Project 2 — Retail Sales & Forecast Dashboard

**Heading:**
```
Retail Sales & Forecast Dashboard
```

**Summary:**
```
A two-part retail analytics workbook: a historical sales performance dashboard and a
forward-looking demand-forecast dashboard projecting revenue by customer segment
through FY2030, with a CAGR-based recommendation engine flagging which segments to
prioritize for future orders.
```

**Key metrics line:**
```
$2.30M total revenue  ·  5,009 orders  ·  37,873 units sold  ·  $458.61 avg. order value
FY2030 forecast: $1.86M (+153.5% vs. FY2017)  ·  Top growth segment: Corporate
```

**Images (in order):**
1. Revenue KPIs, category breakdown, segment summary, top 10 customers/orders
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Sales_Dashboard_1.png`
2. USA sales map and orders/revenue by region
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Sales_Dashboard_2.png`
3. FY2030 forecast KPIs, actual-vs-forecast table, order recommendation by segment
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Forecast_Dashboard_1.png`
4. Actual vs. forecast trend line and over/under-performing segment analysis
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Forecast_Dashboard_2.png`

**Caption / "what to notice":**
```
The recommendation logic compares each segment's FY2014–17 CAGR against the average
across all three segments: Corporate (23.5% CAGR) and Home Office (21.1%) are both
flagged "Increase Orders" for compounding faster than peers, while Consumer (7.6%
CAGR) is held at "Maintain Orders" despite generating the most revenue today —
a case for looking at growth rate, not just current volume.
```

---

### Project 3 — Transportation Analysis Dashboard

**Heading:**
```
Transportation Analysis Dashboard
```

**Summary:**
```
An e-commerce shipping and revenue dashboard analyzing product performance, carrier
efficiency, and transportation cost by mode and route — built on live formulas against
a raw shipment-level dataset, with a documented calculation methodology for how total
shipping cost is derived.
```

**Key metrics line:**
```
$577.6K total revenue  ·  46,099 units sold  ·  $53.5K total shipping cost (9.3% of revenue)
$52.9K total transport cost (mode/route)  ·  5.8 days avg. shipping time
```

**Images (in order):**
1. Full KPI row, product type summary, shipping carrier summary, cost by mode/route tables, revenue-share and units-sold charts
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Transportation_Dashboard_1.png`
2. Revenue vs. shipping cost by carrier, and cost by mode/route charts
   `https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Transportation_Dashboard_2.png`

**Caption / "what to notice":**
```
Skincare drives 42% of revenue share despite being one of three product lines, and
Carrier B — while moving the most volume — also runs the highest per-order shipping
cost ($534.01), a candidate for renegotiation or route rebalancing toward Carrier C.
```

---

## 6. Contact section

**Heading:**
```
Contact
```

**Body text** (placeholder — fill in once you decide what to share):
```
[Add email address]
[Add LinkedIn URL]
[Optional: add phone number]
```

---

## Notes on filling remaining gaps

- No profile photo or downloadable résumé PDF is referenced above — add an **Images** block for a headshot in the hero section, and a **Button** linking to a résumé PDF, if/when you have one (the original `Resume_Vignesh.docx` was removed from the repo).
- All KPI numbers quoted above were read directly off the dashboard screenshots you provided, not invented — double-check them against the live workbook if the underlying data changes before publishing.
- The repo (`https://github.com/lvignesh1961/lvignesh_Portfolio`) is now public, which is what makes the raw image URLs above work in Sites' "Insert Image by URL." Everything else in the repo (the 3 Excel workbooks, the Markdown files) is also publicly visible as a result.
