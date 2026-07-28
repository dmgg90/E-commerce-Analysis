🛒 E-Commerce & Supply Chain Analysis

A structured three-notebook data analysis project built on a global e-commerce dataset. Each notebook covers a distinct business domain — financial performance, customer behaviour, and inventory health — and exports clean DataFrames for Power BI visualisation.

📁 Repository Structure
ecommerce-analysis/
│
├── 01_Sales/
│   └── Sales Analysis.ipynb    ← Revenue, burn rate, cash flow, brand performance
│   └── Plots                   ← Images of the Charts(Brand performance.png,Cashflow.png,MoM Growth.png,Total Revenue vs Net Burn Rate.png)
│
├── 02_Marketing/
│   └── marketing_analysis.ipynb    ← RFM, LTV, segmentation, CAC, ICP
│   └── Plots                   ← Images of the Charts(Annual Marketing Spend vs. Customer Acquisition Cost.png,Customer Distribution by RFM Segment.png,ICP Value Matrix.png,
│                                                      Negative ICP Value Matrix.png,ltv_by_age_group.png,ltv_by_currency.png)
│
├── 03_Inventory/
│   └── Inventory.ipynb    ← Stock health, sell-through, overstock, stockout risk
│   └── Plots                   ← Images of the Charts(Brand Sell-Through Rate & Inventory Breakdown.png,Overstock & Dead Stock Risk Analysis Overview.png,Top 10 Products by Daily Sales Velocity.png,
│                                                       Top Products at Risk of Stockout (Fast Movers).png,inventory_status_countplot.png)
│
└── README.md
📓 Notebooks & KPIs
01 · Sales Report

Business question: How is the business performing financially, and which brands are driving it?

KPI	Definition
Monthly Revenue	Total revenue_usd aggregated by year-month
Gross Burn Rate	cost_usd + shipping_cost_usd per month — total cash spent
Net Burn Rate	(cost_usd + shipping_cost_usd) − revenue_usd — net cash consumed after revenue
Monthly Cash Flow	revenue_usd − (cost_usd + shipping_cost_usd) — retained value per month
YoY Revenue Growth %	Year-over-year revenue change via LAG window function
MoM Revenue Growth %	Month-over-month revenue change, colour-coded green/red
Gross Profit by Brand	revenue_usd − cost_usd − shipping_cost_usd per brand (completed orders only)
Top 3 Brand Revenue Share	Revenue concentration: top 3 brands as % of total revenue

SQL techniques: CTEs, LAG window function, STRFTIME date aggregation, multi-table joins filtered on status = 'completed'

02 · Marketing Analysis

Business question: Who are our customers, what are they worth, and how efficiently are we acquiring them?

KPI	Definition
Recency (R)	Days since customer's last purchase vs. dataset max date
Frequency (F)	Total number of completed transactions per customer
Monetary (M)	Total profit_usd generated per customer (profit, not revenue)
RFM Score	3-digit composite score (e.g. "543") from quintile binning of R, F, M
Customer Segment	6-tier behavioural label: Champions / Loyal / Promising / At Risk / Hibernating / Needs Attention
Average LTV by Age Group	Mean profit_usd per customer across 6 age brackets (<25 through 65+)
Average LTV by Market	Mean profit_usd per customer grouped by currency (country proxy)
CAC (Customer Acquisition Cost)	Total Marketing Spend / New Customers Acquired — calculated per year 2022–2024
ICP Matrix	Count of Champions + Loyal Customers across age × market demographic grid
Negative ICP Matrix	Count of Hibernating / Lost customers across the same age × market grid

SQL techniques: julianday date arithmetic, COALESCE, NULLIF, correlated subqueries, UNION ALL

03 · Inventory Analysis

Business question: Are we stocking the right products, and where is inventory hurting or helping sales?

KPI	Definition
Daily Sales Rate (Velocity)	units_sold_since_restock / days_since_restock — post-restock units per day
Days of Supply Remaining	current_stock / daily_sales_rate — estimated days until stockout at current velocity
Inventory Health Status	4-tier classification: Healthy / Low Stock / Overstocked / Dead Stock
Sell-Through Rate by Brand	units_sold / (units_sold + units_on_hand) × 100 — stock-to-sales conversion efficiency
Overstock Severity	4-tier risk classification: Critical (Dead Stock) / High Risk (>1yr supply) / Moderate / Slight
Stockout Urgency	3-tier alert: Urgent (≤3 days) / High Risk (4–7 days) / Moderate Risk (8–14 days)
Top 10 Products by Velocity	Fastest-moving SKUs by daily sales rate since last restock
Dead Stock Flag	Products with zero post-restock sales after 30+ days

SQL techniques: CTEs, julianday for date arithmetic, NULLIF for division safety, COALESCE, LEFT JOIN with date-filtered transactions, CASE multi-tier classification

🛠️ Stack
Tool	Role
kagglehub	Reproducible dataset download
pandas	Data manipulation and scoring
SQLite	Structured querying across joined tables
seaborn / matplotlib	In-notebook visualisation
Power BI	Dashboard layer (flat file import from exported CSVs)
📊 Dataset

Global E-Commerce and Supply Chain Database — Kaggle

Table	Used in
transactions	All three notebooks
products	Notebook 1 & 3
customers	Notebook 2
marketing_spend	Notebook 2
inventory	Notebook 3
supplier_costs	Notebook 3 (available for cost-of-overstock extension)
returns	Notebook 2 (available for churn extension)
🚀 How to Run
Clone the repository
Install dependencies:
bash
pip install kagglehub pandas matplotlib seaborn scikit-learn
Run notebooks in order: 01 → 02 → 03
Each notebook downloads the dataset automatically via kagglehub — no manual file setup required
To connect to Power BI: export the output DataFrames to CSV and import as flat file sources
