# GitHub Pages Site Content — Vignesh Portfolio
### Styled to match harrisonjansma.com's design system (custom CSS version)

Paste-ready copy and a real, hand-codable design system for a GitHub Pages site (plain HTML/CSS, or Jekyll — either way you have full custom CSS, unlike Google Sites). Image references point to files in [dashboard_screenshots/](dashboard_screenshots/), public on GitHub at `https://github.com/lvignesh1961/lvignesh_Portfolio`.

**Images**: repo is public, so use the raw URL directly as an `<img src>`:
`https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/<filename>` (all 9 verified resolving).

All copy below is unchanged from the original content plan. What's new: an actual CSS token system, real class structure, and HTML snippets you (or Claude Code) can drop straight into `index.html` / `style.css`.

---

## 0. Design system (CSS — this replaces the Google-Sites-only "style notes" from the earlier draft)

### Fonts
```html
<!-- in <head> -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### CSS variables (`style.css`)
```css
:root {
  /* Color */
  --color-bg: #ffffff;
  --color-text: #0a0a0a;
  --color-text-muted: #6b7280;
  --color-border: #e5e7eb;
  --color-surface: #f9fafb;
  --color-accent: #4f46e5;
  --color-accent-hover: #4338ca;
  --color-accent-soft: #eef2ff; /* accent tint for backgrounds/pills */

  /* Type */
  --font-sans: 'Inter', ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, 'SFMono-Regular', Menlo, monospace;

  /* Layout */
  --container-max: 1140px;
  --radius-md: 12px;
  --radius-full: 999px;
  --space-section: 96px;
}

* { box-sizing: border-box; }

body {
  background: var(--color-bg);
  color: var(--color-text);
  font-family: var(--font-sans);
  line-height: 1.6;
  margin: 0;
}

.container {
  max-width: var(--container-max);
  margin: 0 auto;
  padding: 0 24px;
}

section {
  padding: var(--space-section) 0;
}

h1, h2, h3 { font-weight: 700; line-height: 1.1; letter-spacing: -0.02em; margin: 0 0 16px; }
h1 { font-size: clamp(40px, 6vw, 64px); font-weight: 800; }
h2 { font-size: clamp(28px, 4vw, 36px); }
p  { color: var(--color-text); margin: 0 0 16px; }
.muted { color: var(--color-text-muted); }

a { color: var(--color-accent); text-decoration: none; }
a:hover { color: var(--color-accent-hover); }
a.external::after { content: " ↗"; }

/* Section eyebrow */
.eyebrow {
  font-size: 14px;
  font-weight: 600;
  color: var(--color-accent);
  letter-spacing: 0.04em;
  margin-bottom: 8px;
  display: block;
}

/* Buttons */
.btn {
  display: inline-block;
  padding: 12px 24px;
  border-radius: var(--radius-md);
  font-weight: 600;
  font-size: 15px;
}
.btn-primary { background: var(--color-accent); color: #fff; }
.btn-primary:hover { background: var(--color-accent-hover); color: #fff; }
.btn-outline { background: transparent; color: var(--color-text); border: 1px solid var(--color-border); }

/* Stat chips (KPI rows) */
.stat-row {
  display: flex;
  flex-wrap: wrap;
  gap: 32px;
  margin: 24px 0;
}
.stat {
  min-width: 120px;
}
.stat-number { font-size: 28px; font-weight: 700; color: var(--color-text); display: block; }
.stat-label { font-size: 14px; color: var(--color-text-muted); }

/* Tags / pills (skills, tech stacks) */
.tag {
  display: inline-block;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  color: var(--color-text-muted);
  font-size: 13px;
  padding: 6px 12px;
  border-radius: var(--radius-full);
  margin: 0 6px 6px 0;
}

/* Project cards */
.card {
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: 40px;
  margin-bottom: 40px;
  background: var(--color-bg);
}
.card img { width: 100%; border-radius: 8px; margin: 16px 0; display: block; }
.card .caption { font-size: 14px; color: var(--color-text-muted); font-style: italic; margin-top: 12px; }

/* Nav */
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 0;
}
.nav a { color: var(--color-text); margin-left: 24px; font-weight: 500; }
.nav .brand { font-weight: 700; color: var(--color-text); }
```

### Layout components in plain HTML (use as a skeleton)
```html
<header class="container nav">
  <span class="brand">Vignesh | Supply Chain &amp; Data Analytics</span>
  <nav>
    <a href="#home">Home</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
  </nav>
</header>

<section id="hero">
  <div class="container">
    <h1>Vignesh</h1>
    <p class="muted">Supply Chain &amp; Data Analytics Professional</p>
    <p>Supply Chain Analytics professional specializing in demand forecasting, inventory planning, and cross-functional data-driven decisions — skilled in SQL, Python, Power BI, and Microsoft Fabric.</p>
    <a class="btn btn-primary" href="#">Download Résumé</a>
  </div>
</section>
```

Repeat this eyebrow → heading → body pattern for every section:
```html
<section id="skills">
  <div class="container">
    <span class="eyebrow">01 — Skills &amp; Toolset</span>
    <h2>Skills &amp; Technical Toolset</h2>
    <!-- tags go here -->
  </div>
</section>
```

Project cards:
```html
<section id="projects">
  <div class="container">
    <div class="card">
      <span class="eyebrow">03 — Inventory Management Dashboard</span>
      <h2>Inventory Management Dashboard</h2>
      <p><!-- summary --></p>
      <div class="stat-row">
        <div class="stat"><span class="stat-number">1,000</span><span class="stat-label">SKUs tracked</span></div>
        <!-- repeat per metric -->
      </div>
      <img src="https://raw.githubusercontent.com/lvignesh1961/lvignesh_Portfolio/main/dashboard_screenshots/Order_Management_Dashboard_1.png" alt="">
      <p class="caption"><!-- what to notice --></p>
    </div>
  </div>
</section>
```

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

Single scrolling `index.html` with anchor-linked nav (`#skills`, `#projects`, `#contact`) works well and matches the harrisonjansma.com pattern of a one-page scroll with a persistent nav.

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
Supply Chain Analytics professional specializing in demand forecasting, inventory planning, and cross-functional data-driven decisions — skilled in SQL, Python, Power BI, and Microsoft Fabric.
```

**Optional button** (only if you add a PDF resume later):
```
Button text: Download Résumé
Link: [add link to resume PDF once uploaded]
```

*Style note:* once you have real years-of-experience / dashboards-built numbers, add a `.stat-row` under the body text here — this is the strongest single borrow from harrisonjansma.com's hero.

---

## 3. Skills & toolset section

**Section eyebrow:** `01 — Skills & Toolset`

**Section heading:**
```
Skills & Technical Toolset
```

**Body text** (render each item as a `.tag` pill using the CSS above):
```
Power BI · SAP · Power Query · Azure · Microsoft Suite · MS Excel · Google Suite ·
Google Cloud · Jira · Confluence · Tableau · AWS Redshift · Oracle NetSuite ·
Snowflake · Python · Pandas · NumPy · PyTorch · SQL · Linux Environments · Plotly · VBA
```

---

## 4. Experience section

**Section eyebrow:** `02 — Experience`

**Section heading:**
```
Experience
```

**Block 1:**
```
Global Supply Chain Analyst
Motorola Solutions, Vancouver, BC — September 2024 – Present

Built SQL/Python-driven dashboards and portfolio analytics frameworks (Redshift, Tableau) to monitor lifecycle risk across multi-level component and product dependencies, improving decision accuracy by 25%.
Led cross-functional analytics initiatives supporting $5M+ quarterly revenue commitments and 20% faster project delivery.
```

**Block 2:**
```
Supply Planning Analyst
Motorola Solutions, Vancouver, BC — January 2024 – September 2024

Led Excel-based planning tools (VBA/Macros) and SAP automation to optimize inventory accuracy (+15%) and procurement cycle time (-50%).
Built supply-demand risk models supporting $1M in quarterly revenue, and performed MRP/SAP work order planning across capacity and lead-time constraints.
```

**Block 3:**
```
Supply Chain Planner
CIMtech Mfg. Inc., Surrey, BC — August 2021 – January 2024

Developed cost-effective production plans yielding 25% savings in setup/tooling costs, and ERP-based dashboards to improve production visibility.
Improved supplier OTIF from 60% to 68% through capacity planning (demand/forecast, takt time, OEE) as analytics liaison to executive leadership.
```

*Style note:* wrap inline percentage/dollar figures (25%, $5M+, 20%, +15%, -50%, $1M, 60% to 68%) in `<strong style="color:var(--color-accent)">` (or a `.highlight` class) — a light-touch borrow of harrisonjansma.com's habit of visually foregrounding metrics inside body text.

---

## 5. Project sections

Each project section follows the same pattern: eyebrow → heading → one-paragraph summary → key metrics (`.stat-row`) → screenshot(s) → `.caption` ("what to notice"). Wrap each project in a `.card` div per the CSS above.

### Project 1 — Inventory Management Dashboard

**Section eyebrow:** `03 — Inventory Management Dashboard`

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

**Key metrics** (render as `.stat-row` chips):
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

**Section eyebrow:** `04 — Retail Sales & Forecast Dashboard`

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

**Key metrics** (render as `.stat-row` chips):
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

**Section eyebrow:** `05 — Transportation Analysis Dashboard`

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

**Key metrics** (render as `.stat-row` chips):
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

**Section eyebrow:** `06 — Contact`

**Heading:**
```
Contact
```

**Body text:**
```
Email: lvignesh1961@gmail.com
```

```html
<a class="btn btn-primary" href="mailto:lvignesh1961@gmail.com">lvignesh1961@gmail.com</a>
```

---

## Notes on filling remaining gaps

- No profile photo or downloadable résumé PDF is referenced above — add an `<img>` for a headshot in the hero section, and a `.btn` linking to a résumé PDF, if/when you have one (the original `Resume_Vignesh.docx` was removed from the repo).
- All KPI numbers quoted above were read directly off the dashboard screenshots you provided, not invented — double-check them against the live workbook if the underlying data changes before publishing.
- The repo (`https://github.com/lvignesh1961/lvignesh_Portfolio`) is public, which is what makes the raw image URLs above work as direct `<img src>` values.
- Since this is now a hand-coded GitHub Pages site, you have full control: add `@media (max-width: 640px)` rules to stack `.stat-row` items and reduce `--space-section` on mobile, and consider a `prefers-reduced-motion` check if you add any hover transitions on the `.card` elements.
