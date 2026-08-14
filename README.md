# Andes Capital Real Estate — Commercial Performance & Customer Retention Analysis (2023–2024)

A star-schema data model and four-page Power BI dashboard evaluating two years of real estate sales across Colombia and Mexico — built to answer where revenue comes from, which channel drives it, and why customers rarely come back after their first purchase.

## Business Context

The real estate sector needs to evaluate its commercial performance to understand growth, profitability, and customer behavior. This project covers data cleaning, dimensional modeling (star schema), DAX measure design, and an executive summary focused on strategic decision-making, for a real estate business operating in **Bogotá and Mexico City**.

**Core business questions:**
- **Overall performance:** total revenue, sales volume, average ticket, and commission generated
- **Commercial analysis:** which property type, customer segment, and sales channel generate the most value
- **Time trend:** how do sales evolve month over month, and what's the year-over-year (YoY) growth
- **Retention / cohorts:** do customers come back to buy again after their first transaction?

## Key Business Findings

- **$6.01B in total revenue, 8,500 transactions, a $707.35K average ticket, and $200.63M in commissions** across 2023–2024, with revenue trending positively throughout the period.
- **Houses lead the portfolio**, generating 16% more revenue than the next-highest property category — the clearest product preference in the market.
- **The Broker channel dominates**, concentrating almost 3x more transactions and revenue than direct sales — external broker networks are the critical commercial engine of the business.
- **First-time buyers drive the most revenue** of any customer segment, ahead of Investor and High-Net-Worth buyers — evidence of strong acquisition, but not yet matched by retention.
- **Mexico City outperforms Bogotá by 17% in revenue** — a geographic concentration that also signals room for growth in the Bogotá market.
- **Seasonality is predictable and repeats across both years**, with revenue peaks in March, April, September, October, and November.
- **Retention drops sharply after the first purchase.** The cohort matrix shows most customers don't return within the following months — repeat-purchase rates fall well below the initial 100% cohort baseline almost immediately.

## Strategic Insights

- Heavy dependence on first-time buyers points to strong top-of-funnel acquisition, but low maturity in loyalty strategy and Customer Lifetime Value (CLV).
- The external broker network is the critical commercial engine of the business — its incentive structure directly shapes overall revenue.
- Sales behavior is predictable within the year, which makes it possible to plan marketing budgets ahead of the high-demand months rather than reacting to them.

## Recommendations

- **Build a retention program**: post-sale follow-up campaigns targeting the first 3–6 months after a customer's first transaction, when the drop-off is steepest.
- **Strengthen the broker channel**: reinforce commissions and incentives for brokers, given their outsized share of total revenue.
- **Plan around seasonality**: run acquisition campaigns in February and August to be positioned ahead of the March–April and September–November peaks.
- **Prioritize inventory acquisition** for houses in Mexico City, the highest-performing property/city combination.

## Methodology

**Data cleaning & audit (Python)**
- Explicit type casting: `fecha_venta` to `Date`, `precio_venta` and `monto_comision` to numeric
- Primary-key integrity checks: zero duplicates confirmed in `id_cliente` (dim_clientes) and `id_propiedad` (dim_propiedades)
- Null-value audit on all key fact-table fields (price, date, customer ID, property ID) — zero nulls found

**Data modeling (Power BI — Star Schema)**

A dynamic `Dim_Fecha` calendar table was built via DAX and connected to the fact table to support YoY, YTD, and MTD time intelligence without alphabetical month-sorting issues. The model follows a standard star schema, with the fact table `hecho_ventas_propiedades` at the center and single-direction 1:* relationships to `dim_clientes`, `dim_propiedades`, and `Dim_Fecha`.

**DAX measures**
- **Base measures:** Total Revenue, Sales Count, Average Ticket, Total Commission
- **Share-of-total measures:** % share by property type, % share by sales channel (using `ALL()` for filter-context control)
- **Time intelligence:** prior-year revenue, YoY growth %, YTD revenue (`SAMEPERIODLASTYEAR`, `TOTALYTD`)
- **Cohort columns:** first purchase date per customer, cohort month, and sale month — the backbone of the retention matrix
- **Dynamic title measure** for the customer detail page, adapting to the active segment/customer filter

**Dashboard structure (4 pages)**
1. **Executive Overview** — company-wide KPIs, revenue trend with YoY growth overlay, city comparison
2. **Commercial Analysis** — revenue by property type, sales channel, and buyer segment, plus a conditional-formatted matrix of the highest-value property/channel combinations
3. **Cohort Analysis** — a full retention matrix (cohort month × sale month) showing what share of each cohort returns to buy in subsequent months
4. **Customer Detail** — individual customer ranking, full transaction history, and a dynamic title that adapts to the active filter selection

## Repository Structure

```
├── notebooks/
│   └── andes_capital_data_audit.ipynb
├── data/
│   ├── hecho_ventas_propiedades.csv
│   ├── dim_clientes.xlsx
│   └── dim_propiedades.csv
├── dashboard/
│   └── andes_capital_real_estate_dashboard.pbix
├── screenshots/
│   ├── 01_executive_overview.png
│   ├── 02_commercial_analysis.png
│   ├── 03_cohort_retention_matrix.png
│   └── 04_customer_detail.png
├── LICENSE
├── requirements.txt
└── README.md
```

## Tech Stack

Python (pandas, NumPy) · Power BI Desktop · Power Query (M) · DAX

## Limitations

The analysis is descriptive rather than predictive — it explains what happened across cities, channels, and cohorts, but doesn't forecast future demand or customer lifetime value. The dataset covers two cities only (Bogotá and Mexico City), so findings shouldn't be generalized to other markets without further validation.

## Next Steps

- Build a CLV (Customer Lifetime Value) model to quantify the financial upside of improving retention
- Test a post-sale follow-up campaign against a control group to measure its actual impact on repeat-purchase rate
- Extend the geographic footprint of the analysis as the business enters new cities

---

**Author:** William Andrés Bernal Sosa — [GitHub](https://github.com/williambernal-data)
