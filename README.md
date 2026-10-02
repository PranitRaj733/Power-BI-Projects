# Power-BI-Projects

**Project 1 - Commonwealth Games Visualization**
**Description** - It shows total streams as a KPI card, artist type split by genre in a donut, artists by country of origin and streams by debut year in a line chart. Slicers filter by gender and primary language.

**Tech Stack** - 
**For the visual**: Donut chart showing artist type split by genre count.
Clustered bar chart showing artists by country of origin.
Line chart showing streams by debut year.
Slicer for Sex and a tile/advanced slicer for Primary Language.
**DAX**: It relies on implicit aggregations instead SUM of Total Streams and COUNT of Primary Genre and Artist Type.

**Highlights**:
Headline KPI: Total streams (in millions) shown as a card.
Artist mix: A donut chart showing the breakdown by artist type, such as solo, duo or group.
Geographic view: A bar chart of artists by country of origin.
Trend over time: A full-width line chart of streams by debut year showing how artist cohorts perform.
Slicers: Sex and Primary Language so viewers can compare male and female artists or filter by language.

**Project 2 - Inventory & Sales Analysis Dashboard: Project Analysis**
**Description** - This is a single-page Power BI dashboard that gives an overview of sales performance. The Gross Sales and COGS KPI cards come first then the breakdowns by segment, country, product and month and a Discount Band slicer filters everything. It is built on Microsoft's Financial Sample dataset obtained from Kaggle.

**Core Analysis**-
Revenue vs. cost: Gross Sales next to COGS shows the size of the business and its cost burden.
Segment performance: Which customer segments drive the most gross sales.
Product cost mix: Which products account for the largest share of COGS.
Geographic performance: Which countries sell the most units and generate the most profit and whether high volume actually means high profit.
Seasonality: Month-by-month sales patterns.
Discount impact: The Discount Band slicer lets you check how discounting affects every metric on the page.

**Summary** - The project is a well-structured, beginner-to-intermediate Power BI report that shows the basics of data import, type cleaning, KPI cards, multi-dimensional breakdowns and slicer filtering. Adding DAX measures for profit margin and discount % using better chart types and cleaning up the naming would make it more portfolio-ready.

**Project 3 - Student Academic Performance Analysis Dashboard**

**Description** - Single-page Power BI dashboard analyzing how study habits, attendance, sleep, parental education and internet access relate to student academic performance. Uses KPI cards, donut, bar and line charts and an Internet Access slicer to explore a 12-column student dataset.

**Power Query Transformations**
- Loaded the CSV with Csv.Document and promoted the first row to headers
- Set data types: whole number for student_id, decimal for study time, attendance, sleep hours, previous grade and exam score, and text for the categorical fields.
- No custom columns, merges or filtering.

**Key Insights (What the Dashboard Answers)**
- How students are distributed across final grades
- Whether parental education level relates to attendance
- How sleep patterns differ between genders
- How study time is spread across the student group
- Whether internet access changes any of the patterns above.


**Project 4 - Retail Sales Dashboard for Analysis**
**Description Summary**
This is a two-page Power BI report built on a single flat fact table — `E‑commerce_Delivery_Shipping_Data_2026` — containing 50,000 order-level records across 2026 (Jan–Dec), spanning 17 countries, 10 warehouse cities, 12 product categories, 8 carriers and 5 shipping methods. The report is a logistics/delivery-performance analytics dashboard layered on top of an e-commerce order dataset: it blends order economics (order value, shipping cost, cost-to-serve) with fulfilment performance (delivery variance, delays, returns, customer rating).

**Core Tech Stack**
The build shows solid fundamentals — Power Query ingestion, DAX calculated columns, DAX measures with dependency chaining (`Unique_Cust → Returned_Orders → Return Rate`) and a two-page layout that separates "order status/economics" from "delivery pattern/quality". It also has some rough edges typical of a first strong portfolio piece (an empty calculated column, one broken measure, some implicit vs. explicit measure inconsistency and a default un-customized theme)
**Data Source & Model**
Item - Detail
Source- CSV (`E-commerce_Delivery_Shipping_Data_2026.csv`), loaded via Power Query
Grain	- One row = one order
Row count - 	50,000
Native column - 30
Model tables - 1 fact table + 2 auto-generated Power BI date hierarchy tables
Date range - 2026‑01‑01 to 2026‑12‑31

**Data Preparation (Power Query / ETL)**
The M query applied:
1.Load CSV with explicit delimiter/encoding settings.
2.Promote headers.
3.Explicitly type all 29 native columns (text, date, number, Int64 — not left as "Any").
4.Add one custom column in Power Query itself: `Delivery Variance = actual_delivery_days − promised_delivery_days`.

**Calculated Columns (Power Query + DAX)**
Power Query (M) — 1 column
`Delivery Variance` = `[actual_delivery_days] - [promised_delivery_days]` → used in report ✅ (feeds `Delivery Variance Category`)

**DAX calculated columns on the fact table — 8 columns**
Delivery Variance Category` = `IF([Delivery Variance] <= 0, "Arrival Before ETA", "Delayed Delivery") → used in visuals ✅
Cost-To-Serve = shipping_cost_usd / order_value_usd → used in visuals ✅
Shipment_Flagging = IF(shipping_cost_usd > order_value_usd, "Uneconomic Orders", "Economic Orders") → used in visuals ✅
Shipping_Flag = IF(shipping_cost_usd > order_value_usd, "Shipping Loss", "Profit Shipment") → used in visuals ✅
Order_Value Density =DIVIDE(order_value_usd, product_weight_kg) → not placed directly on canvas, but feeds the Median_Density measure.
Order_Year = `YEAR(order_date)
Order_Month = MONTH(order_date)

**DAX Measures Incorporated**
Average_Product Weight = AVERAGE(product_weight_kg)
Avg Distance = AVERAGE(distance_km)
Median_Density = AVERAGE(Order_Value Density)
Unique_Cust = DISTINCTCOUNT(order_id)
Average_Cust Rating = AVERAGE(customer_rating)
Returned_Orders = CALCULATE([Unique_Cust], return_requested = "Yes")
Return Rate = DIVIDE([Returned_Orders], [Unique_Cust]) * 100 → used ✅ (KPI card + table, Page 2)
Total_Shipping Cost = SUM(shipping_cost_usd)
Average Delivery_Delay = AVERAGE(delivery_delay_days)
Avg Promised_Delivery TAT = AVERAGE(promised_delivery_days)

**Key Business Insights Findable in the Data**

**1.Late deliveries are the norm, not the exception:** ~56% of all orders are flagged as late — a headline KPI worth surfacing more prominently (currently there's no single "on-time %" card).
**2.Shipping economics are inverted for over half of all orders:** ~51.7% of shipments cost more to ship than the order is worth (Shipping_Flag = "Shipping Loss") and the average Cost-to-Serve ratio is ~3.1x (shipping cost is, on average, over 3x the order value). This is the single most striking number in the dataset and is currently under-surfaced (it only appears split across a pie chart and a column chart, with no headline KPI card).
**3.Return Rate-** sits around 17%, with Product Defect, Damaged Package, and "Not as Described" as the top reasons — an unexploited dimension (see gap analysis above).
**4.Average customer rating-** is 3.37 / 5 — middling, consistent with the delay/return patterns above.
**5**.12 product categories, 8 carriers, 5 shipping methods, and 4 delivery-status states give enough categorical richness to support the matrix/decomposition additions recommended above.

**Add 1–2 exported screenshots of each page (File → Export → Export to Image/PDF in Power BI Desktop) so the README renders visually on GitHub without requiring a Power BI license to view.**


**Project 5 - Beverage Dashboard Analytics — Coffee Quality & Price Benchmark**
**Description - An interactive Power BI dashboard that models a multi-table coffee-lot benchmark dataset to explain what actually drives coffee quality, price and where the underlying data can and cannot be trusted.

**Introduction** - Global coffee buyers, roasters and quality-control teams routinely make sourcing and pricing decisions off spreadsheets that mix duplicate records, incomplete sensor readings and loosely related quality metrics — this dashboard was built to show how a properly modeled, relationship-driven Power BI report turns that same raw data into a defensible, decision-ready view of quality, price and data reliability.

**Project Status**
This documents the current build: a single, densely-instrumented report page, **"Price & Target Analysis,"** containing 17 visuals (KPI cards, cards, a scatter chart, clustered bar charts, a donut chart, a column chart, a year slicer and a bookmark-driven navigation button). It is the analytical core of a planned five-page dashboard; remaining pages (Data Quality Overview, Farm & Environment Drivers, Sensory Profile and a Feature Dictionary reference page) are scoped and in progress. Framing the README around what is actually built, rather than the full roadmap is intentional — a reviewer should never have to guess what's real versus planned.

**Tech-Stack**
**Power BI Desktop** — primary authoring tool for data modeling, DAX, and report design
**Power Query (M)** — data ingestion, cleaning, type correction, conditional-column logic and table merges from four source CSV files
**DAX (Data Analysis Expressions)** — custom measures for KPIs, percentages, train/test benchmarking and dynamic targets (e.g., training-set average/median used as live goal lines)
**Star-schema-style data modeling** — a Dim_Lots dimension table related 1-to-many to a Fact_CoffeeBenchmark fact table on lot_id, with a record_type attribute merged in (not modeled as a redundant relationship) to track duplicate/revision records at source
**Power BI KPI visual with dynamic targets** — quality and price benchmarks calculated from the training split of the data rather than hardcoded constants
**Power BI bookmarks & navigation (action buttons)** — in-report navigation groundwork for the multi-page rollout
**CSV** — raw source files, version-controlled independently of the .pbix

**What's on the "Price & Target Analysis" Page**
**KPI visuals:** Average Quality Score vs. a data-derived Quality Target; Average Price vs. a data-derived Price Target — both benchmarked against the training split rather than an arbitrary fixed number.
**Scatter chart:** Quality Score vs. Price per Kg, colored by quality grade, sized by lot size — surfaces that quality alone is a weak price predictor.
**Clustered bar chart:** Total Records by country of origin.
**Clustered bar chart:** Sensory profile comparison (acidity, sweetness, body, aroma, flavor) — Arabica vs. Robusta.
**Column chart:** Average Quality Score by country of origin.
**Donut chart:** Quality grade distribution across the portfolio.
**Cards:** Total Unique Lots, Average Quality by Altitude Band, Average Altitude and supporting summary metrics.
**Slicer:** Harvest calendar year, for trend-level filtering.
**Navigation button:** A bookmark-linked "Back" control, laying the interaction pattern for the multi-page version.

**How This Dashboard Helps in Core Business Scenarios**

**Sourcing decisions:** Buyers can immediately see whether a country or altitude band is delivering quality consistent with its price point instead of relying on anecdotal supplier reputation.
**Pricing strategy validation:** The quality-vs-price scatter chart exposes that grade alone explains only part of price variation, prompting a review of what else (e.g., buyer segment, certification) should factor into pricing models — a direct input into commercial strategy discussions.
**Target-setting with real benchmarks:** Because the Quality and Price targets are calculated from the training-split average/median rather than hardcoded, the KPI visuals stay valid as new harvest data arrives — no manual re-baselining required each season.
**Risk flagging on data reliability:** Surfacing duplicate/revision records and incomplete fields up front means downstream averages and forecasts aren't silently skewed by data-entry noise — a control most dashboards skip entirely.
**Cross-functional alignment:** A single page serves procurement (country/altitude view), quality control (sensory and grade breakdown) and finance (price benchmarking) without needing three separate reports.
**Scalable governance:** The relationship model (lot-level dimension to record-level fact, with duplicate/revision tracking) mirrors how real commodity-trading and QA systems are structured, so the same pattern extends cleanly as more harvest years or origins are added.


**Why This Project Reflects Senior-Analyst-Level Work**
Modeled data at the correct grain instead of flattening everything into one table — a deliberate one-to-many relationship, not a default Power BI import
Replaced arbitrary target values with targets calculated directly from the training split showing an understanding of how benchmarks should behave in an evolving dataset
Investigated and explicitly handled data-quality issues (duplicate/revision records, missing environmental readings) rather than letting them quietly distort KPIs
Chose statistically appropriate aggregations (e.g., median over mean for a right-skewed price field) rather than defaulting to averages everywhere
Built the report with a navigation pattern intended for a multi-page rollout showing planning beyond a single static screen


**Roadmap**

**Data Quality Overview page** — record-type breakdown, null-rate audit, train/test balance check.
**Farm & Environment Drivers page** — altitude, processing method, defect rate, and farm-type analysis.
**Sensory Profile page** — Full five-attribute sensory breakdown by species and quality grade.
**Feature Dictionary reference page** — intentionally disconnected documentation table explaining column semantics and prediction-timing risk (pre-harvest vs. outcome-stage features)








