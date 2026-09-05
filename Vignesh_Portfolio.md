# Vignesh Portfolio

Personal info sourced from [Resume_Vignesh.docx](Resume_Vignesh.docx); project details sourced directly from the workbook contents of [Inventory_Management_Dashboard_FINAL.xlsx](Inventory_Management_Dashboard_FINAL.xlsx), [Retail Sales & Forecast Dashboard_Final.xlsx](Retail%20Sales%20%26%20Forecast%20Dashboard_Final.xlsx), and [Transportation_Analysis_Dashboard_Refined_FINAL.xlsx](Transportation_Analysis_Dashboard_Refined_FINAL.xlsx). Structured on the same reference format as [Website clone.md](Website%20clone.md) (per-project sections instead of per-site sections).

# About

**Vignesh** — Data-driven Supply Chain Analytics professional with 6+ years of experience in demand forecasting, inventory planning, and supply-demand analytics supporting revenue and operational decisions across large-scale manufacturing and supply chain environments. Skilled in SQL and Python for analyzing historical performance and developing reporting tools that improve inventory accuracy and optimization. Experienced partnering cross-functionally with finance, operations, and supply chain teams to drive data-informed planning. Currently expanding expertise in Power BI and the Microsoft Fabric ecosystem.

**Skills & technical toolset:** Power BI, SAP, Power Query, Azure, Microsoft Suite, MS Excel, Google Suite, Google Cloud, Jira, Confluence, Tableau, AWS Redshift, Oracle NetSuite, Snowflake, Python, Pandas, NumPy, PyTorch, SQL, Linux Environments, Plotly, VBA.

**Career-related experience:**

- **Global Supply Chain Analyst** — Motorola Solutions, Vancouver, BC (Sept 2024 – Present). Partners with cross-functional stakeholders to translate planning requirements into SQL (AWS Redshift) and Python-based Tableau dashboards, improving inventory/financial decision accuracy by 25%; builds SQL-based ETL pipelines and KPI/OKR dashboards monitoring lifecycle risk across 80,000+ components; develops S&OP/demand-planning analytics frameworks (Last Time Buy, EOL exposure, ramp curves); leads cross-functional analytics initiatives via Jira/Confluence improving delivery timelines by 20%; documents supply-risk and scenario modeling supporting $5M+ quarterly revenue commitments.
- **Supply Planning Analyst** — Motorola Solutions, Vancouver, BC (Jan 2024 – Sept 2024). Built Excel/VBA planning tools improving inventory accuracy by 15%; automated SAP purchase-order/delivery workflows cutting procurement cycle time by 50%; built supply-demand risk models and dashboards supporting $1M in quarterly revenue commitments; executed MRP logic and SAP work orders for aggregate/mix planning.
- **Supply Chain Planner** — CIMtech Mfg. Inc., Surrey, BC (Aug 2021 – Jan 2024). Developed cost-effective production plans yielding 25% savings in setup/tooling costs; improved supplier OTIF from 60% to 68%; built ERP-based dashboards for production/planning KPIs; ran capacity planning using demand/forecast, takt time, and OEE; served as analytics liaison between operations and executive leadership.

# Project 1: Inventory Management Dashboard

`Inventory_Management_Dashboard_FINAL.xlsx` — an Excel-based inventory analytics workbook for an e-grocery operation, built for SKU-level stock health, reorder planning, and ABC-class inventory control.

## Workbook structure

- **Order Management Dashboard** — reorder-focused view.
- **Inventory Dashboard** — overall stock-health view.
- **Inventory Data E-Grocery** — the underlying SKU-level dataset (raw source rows).
- **Sheet1** — supporting sheet.
- **Lists** *(hidden)* — lookup/reference lists (e.g. dropdown source values).

## Source data (per-SKU fields)

`SKU_ID, SKU_Name, Category, ABC_Class, Supplier_ID, Supplier_Name, Warehouse_ID, Warehouse_Location, Batch_ID, Received_Date, Last_Purchase_Date, Expiry_Date, Stock_Age_Days, Quantity_On_Hand, Quantity_Reserved, Quantity_Committed, Damaged_Qty, Returns_Qty, Avg_Daily_Sales, Forecast_Next_30d, Days_of_Inventory, Reorder_Point, Safety_Stock, Lead_Time_Days, Unit_Cost_USD, Last_Purchase_Price_USD, Total_Inventory_Value_USD, SKU_Churn_Rate, Order_Frequency_per_month, Supplier_OnTime_Pct, FIFO_FEFO, Inventory_Status, Count_Variance, Audit_Date, Audit_Variance_Pct, Demand_Forecast_Accuracy_Pct, Notes` — plus formula-driven helper columns: `ValueRankHelper, NeedsReorder, SuggestedReorderQty, SuggestedReorderValue, ReorderUrgencyHelper`.

Categories span Pantry, Fresh Produce, Dairy, Bakery, Meat, Seafood, Frozen, Beverages, Household, and Personal products, sourced from multiple suppliers and warehouses (Toronto, Vancouver, etc.).

## KPIs & sections

- **Headline metrics**: Total SKUs, Total Qty On Hand, Total Inventory Value, Avg Days of Inventory, Avg Supplier On-Time %, SKUs Needing Reorder.
- **Product Summary by Category** — rollup table.
- **Inventory Status Breakdown** — SKU counts by status (e.g. *Healthy*, *High return rate*).
- **ABC Analysis – Class Summary (Pareto 80/20 Rule)** — Class A = highest-value ~80% of spend (tightest control, weekly cycle counts, statistical safety stock); Class B = next ~15% (monthly review); Class C = remaining ~5% (quarterly review, simple min-max).
- **Reorder Point Summary – Top 20 Most Urgent SKUs (Company-wide)** — ranked by shortfall vs. reorder point, where `Suggested Qty = MAX(Forecast_Next_30d − Qty On Hand + Safety Stock, 0)` — the quantity needed to cover the next 30 days of forecast demand plus safety buffer.
- **Stock Aging & Turnover (Slow-Moving / Dead Stock)** — inventory value broken out by stock-age bucket.

## Charts (6 total, all bar charts)

1. **Reorder Recommendation by Category** — Suggested Reorder Qty.
2. **Category Performance: Forecast Accuracy vs. Portfolio Average** — Avg Forecast Accuracy vs. portfolio-average reference line.
3. **Supplier On-Time % vs. Lead Time Variability** — Avg On-Time % against Lead Time Std Dev (days).
4. **Inventory Value by Category** — Value (USD).
5. **Top 20 SKUs — Value & Cumulative % (Pareto)** — Value (USD) bars with a cumulative-% overlay, the classic Pareto chart.
6. **Inventory Value by Stock Age Bucket** — Value (USD).

# Project 2: Retail Sales & Forecast Dashboard

`Retail Sales & Forecast Dashboard_Final.xlsx` — a two-part retail analytics workbook: a historical sales dashboard and a forward-looking demand-forecast/segment-prioritization dashboard, through FY2030.

## Workbook structure

- **Sales dashboard** — historical sales performance.
- **Forecast Dashboard** — segment-level forecast and order-priority recommendations.
- **Retails Order Full Dataset** — raw order-level dataset.
- **Calc_Base, Calc_Category, Calc_Segment, Calc_Region, Calc_State, Calc_Customers, Calc_Orders, Calc_Top10, Calc_Recommend** *(all hidden)* — staging/calculation sheets feeding the two dashboards.
- **Macro Guide** — documentation sheet for any workbook macros.

## Source data (order-level fields)

`Row ID, Order Date, Ship Date, Ship Mode, Customer ID, Customer Name, Segment, Country, City, State, Postal Code, Region, Retail Sales People, Product ID, Category, Sub-Category, Product Name` — plus revenue/quantity metrics.

## Sales dashboard — KPIs & sections

- **Headline metrics**: Total Revenue, Total Orders, Total Units Sold, Avg Order Value.
- **1. Products Sold by Product Type (Category)** — Category, Quantity Sold, Revenue, Line Items, % of Revenue.
- **2. Sales Segment Summary** — Segment, Distinct Orders, Avg Order Value.
- **3. Top 10 Customers by Sales Revenue** — ranked list.
- **4. Top 10 Orders by Sales Revenue** — Order ID, Segment/Region.
- **5. USA Sales Map — Orders & Revenue by Region/State**.

## Forecast dashboard — KPIs & sections

- **Headline metrics**: FY2017 Actual Total Sales, FY2030 Forecast Total Sales, Projected Growth FY17–FY30, Top Growth Segment.
- **1. Sales Forecast by Segment — Actual (FY2014–17) vs. Forecast (FY2018–30)** for Consumer, Corporate, and Home Office segments.
- **2. Order Recommendation — Which Segment to Prioritize Through FY2030** — FY2017 Actual, FY2030 Forecast, Growth FY17–FY30, CAGR FY14–FY17, Recommendation. Recommendation logic: a segment is flagged *Increase Orders* when its FY2014–17 CAGR is at/above the average CAGR across all three segments (compounding faster than peers, steepest projected FY2030 revenue); *Reduce Orders* flags any segment with negative historical CAGR; all others are *Maintain Orders* — with a note to re-validate against actual demand signals before committing purchase orders that far out.
- **3. Over-Performing vs. Under-Performing Segments** — Avg CAGR (all segments), vs. Average, Performance Flag.

## Charts (8 total)

1. **Revenue & Quantity by Product Category** — bar.
2. **Revenue Share by Segment** — pie.
3. **Top 10 Customers by Revenue** — bar.
4. **Orders & Revenue by US Region** — bar.
5. **Segment Sales: Actual vs. Forecast (through FY2030)** — line, by fiscal year.
6. **CAGR vs. Average — Over/Under-performing Segments** — bar.
7. **Recommended Order Priority — FY2030 Forecast Revenue by Segment** — bar.
8. **Order Revenue by State** — map/geo chart.

# Project 3: Transportation Analysis Dashboard

`Transportation_Analysis_Dashboard_Refined_FINAL.xlsx` — an e-commerce shipping & revenue dashboard analyzing product performance, carrier efficiency, and transportation cost by mode/route.

## Workbook structure

- **Transportation dashboard** — the single analytical view.
- **supply_chain_data_TR** — the raw source dataset (explicitly cited on the dashboard: *"Source: supply_chain_data_TR"*).

## Source data (fields)

`Product type, SKU, Price, Availability, Number of products sold, Revenue generated, Customer demographics, Stock levels, Lead times, Order quantities, Shipping times, Shipping carriers, Shipping costs, Supplier name, Location, Lead time, Production volumes, Manufacturing lead time` — product types include Cosmetics, Haircare, and Skincare; carriers are Carrier A/B/C; transport modes are Air, Rail, Road, Sea across Route A/B/C.

## KPIs & sections

- **Headline metrics**: Total Revenue, Total Units Sold, Total Shipping Cost, Shipping Cost % of Revenue, Total Transport Cost (Mode/Route), Avg Shipping Time (days).
- **1. Product Type Summary** — Units Sold, Revenue Generated, Avg. Price, Revenue Share % (per product type, with a Total row).
- **2. Shipping Carrier Summary** — Shipments, Total Shipping Cost, Avg. Shipping Cost/Order, Avg. Shipping Time (days), Shipping Cost % of Revenue (per carrier, with a Total/Avg. row).
- **3. Transportation Cost by Mode & Route** — Total Cost and Avg. Cost/Shipment, broken out by Transport Mode and by Route.
- A documented calculation note: "Total Shipping Cost" (KPI and Section 2) combines per-order carrier shipping cost (column M) plus per-shipment route/transportation cost (column X); Section 3 breaks out the column-X transportation cost alone by Mode and Route. All figures are calculated live via `SUMIF`/`AVERAGEIF` formulas against the `supply_chain_data_TR` sheet and update automatically when source data changes.

## Charts (6 total)

1. **Revenue Share by Product Type** — pie.
2. **Units Sold by Product Type** — bar.
3. **Revenue vs. Total Shipping Cost by Carrier** — bar.
4. **Avg. Shipping Time by Carrier (days)** — bar.
5. **Total Transportation Cost by Mode** — bar.
6. **Total Transportation Cost by Route** — bar.

# Common patterns across all three dashboards

- Built entirely in **Excel** (no Power BI/Tableau in these three files), using native chart objects driven live by formulas against a raw data sheet rather than pivot tables — each dashboard sheet is a formula-computed KPI/summary layer sitting on top of a full transactional dataset sheet.
- Consistent structure: headline KPI row → numbered breakdown sections (by category/segment/carrier/etc.) → supporting charts — the same "metrics up top, detail below" layout across all three.
- Heavy use of hidden calculation/staging sheets (`Lists`, `Calc_*`) to keep the visible dashboard sheets clean while formulas do the aggregation work.
- Recurring analytical techniques: ABC/Pareto analysis (Inventory), CAGR-based segment forecasting (Retail), and cost-driver decomposition by mode/route/carrier (Transportation) — all reflecting the resume's emphasis on demand forecasting, inventory optimization, and supply-demand risk analytics.
