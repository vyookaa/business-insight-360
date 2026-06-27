# Business Insight 360 — AtliQ Hardware Power BI Dashboard

An end-to-end business intelligence dashboard built in Power BI, consolidating **Finance, Sales, Marketing, Supply Chain, and Executive** views into a single decision-support tool for AtliQ Hardware, a consumer electronics distributor operating across APAC, NA, EU, and LATAM.

The goal wasn't just to visualize data — it was to trace *why* AtliQ's profitability was breaking down despite strong revenue, and surface that story across five functional lenses a real leadership team would actually use.


---

## The Business Problem

AtliQ Hardware closed FY2020 with **$267.98M in Net Sales** — a 140.6% jump over benchmark — but **Net Profit % came in at -0.85%**, missing the 2.21% benchmark by **138.7%**. Revenue was growing fast and profitability was collapsing at the same time. Finance had the P&L, Sales had customer numbers, Supply Chain had forecast data — but no one view connected them. This dashboard was built to close that gap.

---

## Key Insights

A few things the data surfaced that I'd flag to leadership first:

- **Profitability is a cost problem, not a pricing problem.** Gross Margin % held steady at 37.10% (close to the 41.2% benchmark), but Net Profit % went negative. The gap sits in **Operational Expense**, which grew 134% against benchmark — margin was protected at the product level but lost below the line.

- **Every single segment is profit-negative.** Networking, Peripherals, Accessories, Notebook, Storage, and Desktop all show negative Net Profit %, with Storage (-1.78%) and Desktop (-2.88%) the worst performers despite reasonable Gross Margins (36–36.5%). This points to a fixed-cost or allocation issue spread across the whole portfolio rather than one bad product line.

- **Forecast accuracy collapsed in Nov–Dec 2019**, dropping to **13.40%** and **31.91%** against a steady ~80% baseline, with Net Error spiking to **$1.31M**. This is the single sharpest anomaly in the dataset and the likely root cause behind downstream inventory/fulfillment issues — worth a dedicated RCA rather than a dashboard footnote.

- **Top accounts are a margin risk, not just a revenue source.** Amazon and AtliQ Exclusive together drive ~30% of revenue contribution, but both show the steepest negative GM% deltas (-8.68% and -5.42%) of any major account — concentration risk is building on the customers with the *worst* trending margins.

- **Regional performance is inconsistent under a flat headline.** India and ROA post negative Net Profit % (-14.73%, 8.87% positive but high error), while NA and NE are barely positive — the "All" view in Executive hides that most regions are individually underwater.

---

## Dashboard Views

| View | Focus |
|---|---|
| **Executive** | Cross-functional KPI summary, top customers/products, sub-region performance, market share |
| **Finance** | Full P&L with benchmark/LY variance, COGS breakdown, regional Net Sales trend |
| **Sales** | Customer-level GM$ and GM% performance, NS vs GM% bubble analysis by region |
| **Marketing** | Segment profitability (NS vs NP%), Net Sales/COGS/Gross Margin bifurcation |
| **Supply Chain** | Forecast Accuracy %, Net Error trend, customer and product-segment risk flags |

---

## Tools & Techniques

- **Power BI Desktop** — data modeling, DAX, Power Query, report design
- **Star-schema data model** — fact table for transactions, dimension tables for Customer, Product, Region, Date
- **DAX measures** for benchmark/target variance logic (e.g. `BM % Variance`, `Forecast Accuracy %`, `Net Error %`)
- **Bookmarks & drill-through** for the Executive → Sub-Region → Customer navigation path
- **Risk flagging logic** (EI / OOS indicators) built as a calculated column off Forecast Accuracy thresholds

### Sample DAX

```dax
Net Profit % =
DIVIDE([Net Profit $], [Net Sales $], 0)

Forecast Accuracy % =
1 - DIVIDE([Absolute Error $], [Net Sales $], 0)

GM % Variance vs BM =
DIVIDE([Gross Margin %] - [Gross Margin % BM], [Gross Margin % BM], 0)
```

---

## Data Model

*(insert a screenshot of your relationships view here — `docs/data-model.png`)*

Fact table (`Transactions`) connects to `Dim_Customer`, `Dim_Product`, `Dim_Region`, and `Dim_Date` on standard 1-to-many relationships, with a separate `Benchmark` table joined on Product/Region/Period for the variance calculations used throughout the Executive and Finance views.

---

## What I'd Do Next

- Build a dedicated **RCA layer** for the Nov–Dec 2019 forecast accuracy collapse — the dashboard shows *that* it happened, not *why*
- Add a **cost-allocation breakdown** within Operational Expense, since that's where the real profit leak is, and the current model only shows it at the aggregate level
- Move static benchmark tables into a proper **what-if parameter** so target scenarios can be adjusted without rebuilding the model
- Automate data refresh from source extracts instead of static load, to make this usable as a living report rather than a point-in-time snapshot

---

## Files in This Repo

```
├── README.md
├── /screenshots          → static views of all 5 dashboard pages
├── /pbix                 → business-insight-360.pbix
└── /docs
    └── data-model.png    → relationships diagram
    └── dax-measures.md   → full measure list with explanations
```

---

*Built as part of independent data analytics practice, using the AtliQ Hardware case study dataset.*
