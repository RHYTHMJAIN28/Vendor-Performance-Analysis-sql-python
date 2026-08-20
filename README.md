\### Vendor Performance Analysis



Analyzed 15.6M+ purchase, sales, and freight records across 125 vendors and 10,600+ vendor-brand relationships to identify underperforming vendors, pricing inefficiencies, and cost-saving opportunities in a retail supply chain.



!\[Top Vendors by Sales](assets/top\_vendors.png)



\## Problem



A retail distributor works with 100+ vendors across thousands of SKUs. Leadership had no consolidated view of which vendors were actually driving profit versus tying up capital in slow-moving inventory, and freight costs were being tracked manually per invoice with no vendor-level rollup.



\## Approach



1\. \*\*Data engineering\*\* — Ingested six raw CSV sources (purchases, sales, purchase prices, vendor invoices, beginning/ending inventory — 15.6M rows total) into a normalized SQLite database using a chunked, memory-safe Python ingestion pipeline.

2\. \*\*SQL aggregation\*\* — Built a consolidated vendor × brand summary table via multi-CTE SQL joins across purchases, sales, and freight data.

3\. \*\*Statistical analysis\*\* — Used confidence intervals and Welch's t-test to determine whether the profit-margin gap between high-sales and low-sales vendors was statistically significant, not just noise.

4\. \*\*Data validation\*\* — Ran the full pipeline against the real 2GB dataset and caught two production-grade bugs that only surfaced with real data (see below).

5\. \*\*BI layer\*\* — Built a 4-page interactive Power BI dashboard on top of the verified summary table for stakeholder reporting.



\## Key Findings



\- \*\*Vendor concentration:\*\* the top 10 vendors account for the large majority of purchase volume (Pareto analysis below) — a small vendor set drives most of the business.

\- \*\*Statistically significant profit gap:\*\* a Welch's t-test confirmed high-sales-volume vendors and low-sales-volume vendors have significantly different profit margins (p < 0.05), not explained by chance.

\- \*\*Bulk-purchase discount effect:\*\* larger order sizes correlate with meaningfully lower per-unit purchase prices, quantifying the leverage bigger vendors get from bulk orders.

\- \*\*178 dead-stock brand relationships:\*\* identified vendor-brand pairs with purchase activity but zero sales — $282K in capital sitting in inventory with no turnover.



!\[Vendor Purchase Contribution (Pareto)](assets/pareto\_vendors.png)



\## Power BI Dashboard



A 4-page interactive Power BI dashboard built on the verified dataset above — KPI scorecards, vendor rankings, freight-cost tracking, and a vendor drillthrough page.



!\[Dashboard Demo](assets/dashboard-demo.gif)



\*\*Executive Summary\*\* — KPIs, Top 10 Vendors, Pareto contribution chart

!\[Executive Summary](assets/dashboard-executive-summary.png)



\*\*Freight \& Inventory\*\* — freight cost tracking, dead-stock table ($282K in never-sold inventory)

!\[Freight \& Inventory](assets/dashboard-freight-inventory.png)



\*\*Vendor Rankings\*\* — full vendor matrix with conditional formatting, sales-vs-margin scatter

!\[Vendor Rankings](assets/dashboard-vendor-rankings.png)



\*\*Vendor Detail\*\* — drillthrough page filtered to a single vendor

!\[Vendor Detail](assets/dashboard-vendor-detail.png)



The `.pbix` file (`PowerBiDashboard.pbix`) is included in this repo — download and open in Power BI Desktop to explore interactively.



\*\*A data-quality bug caught while building it:\*\* the freight cost field is stored at vendor grain but the underlying table is at vendor×brand grain — a naive `SUM()` in a DAX measure inflated total freight cost by roughly 400x ($1.64M real vs. $654M naive). Caught by cross-checking against the raw `vendor\_invoice` source data, and fixed with a de-duplicated `SUMX(VALUES(...), CALCULATE(MAX(...)))` measure. The same grouping issue also appeared in table visuals grouped by product description instead of brand ID, and was fixed the same way.



\## Data Quality Issues Found \& Fixed



Real-world data surfaced bugs that don't show up in a code review — the kind of validation that matters before numbers go in front of stakeholders:



| Issue | Impact | Fix |

|---|---|---|

| Purchase-price join matched on `Brand` only | Risked inflated purchase totals for brands carried by multiple vendors | Joined on `Brand + VendorNumber` |

| `ProfitMargin` divided by zero for never-sold brands | Silently produced `-inf`, corrupting every downstream mean/std/CI | Guarded with a conditional (`NaN` when no sales occurred) |

| Freight cost stored at vendor grain but joined into a vendor×brand table | Naively summing freight cost inflated the true total by \~400x (verified against source data) | Fixed in Python with a documented aggregation rule; fixed in Power BI with a de-duplicated DAX measure |

| `pandas.to\_sql(if\_exists='replace')` silently dropped a manually defined `PRIMARY KEY` | Table integrity constraint was a no-op | Reordered to `DROP TABLE` → `CREATE TABLE` (with PK) → `to\_sql(if\_exists='append')` |



\## Tech Stack



`Python` · `pandas` · `NumPy` · `SciPy` (hypothesis testing) · `SQLite` / `SQLAlchemy` · `Matplotlib` · `Seaborn` · `Jupyter` · `Power BI` (DAX, Power Query)



\## Repository Structure



```

├── assets/

│ ├── dashboard-executive-summary.png

│ ├── dashboard-freight-inventory.png

│ ├── dashboard-vendor-rankings.png

│ ├── dashboard-vendor-detail.png

│ ├── dashboard-demo.gif

│ ├── top\_vendors.png

│ └── pareto\_vendors.png

├── data/

│ └── vendor\_sales\_compressed.csv # pre-built summary table (10,648 rows)

├── scripts/

│ └── ingest\_db.ipynb # chunked CSV -> SQLite ingestion

├── notebooks/

│ ├── Exploratory Data Analysis.ipynb

│ └── Vendor Performance Analysis.ipynb

├── report/

│ └── Vendor Performance Report.pdf

├── PowerBiDashboard.pbix # open in Power BI Desktop

└── requirements.txt

```



\## Run It



```bash

pip install -r requirements.txt

jupyter notebook scripts/ingest\_db.ipynb                        # build inventory.db (add raw CSVs to data/ first)

jupyter notebook "notebooks/Exploratory Data Analysis.ipynb"    # build the summary table

jupyter notebook "notebooks/Vendor Performance Analysis.ipynb"  # run the analysis

```



Or skip straight to the analysis using the pre-built `data/vendor\_sales\_compressed.csv`.



For the dashboard: open `PowerBiDashboard.pbix` in Power BI Desktop.



\## Author



Rhythm Jain — rhythmjain2021@gmail.com



License: MIT

