# NST DVA Capstone 2 — Olist Customer Churn Analysis

> **Newton School of Technology | Data Visualization & Analytics**
> A 2-week industry simulation capstone using Python, GitHub, and Tableau to convert raw e-commerce data into actionable customer retention intelligence.

---

### Quick Start

If you are working locally:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

If you are working in Google Colab:

- Upload or sync the notebooks from `notebooks/`
- Keep the final `.ipynb` files committed to GitHub
- Export any cleaned datasets into `data/processed/`

---

## Project Overview

| Field | Details |
|---|---|
| **Project Title** | Olist Customer Churn Analysis — Understanding Why Customers Do Not Return and What Olist Can Do About It |
| **Sector** | E-Commerce / Retail |
| **Team ID** | Section D, Group 9 |
| **Section** | Section D |
| **Faculty Mentor** | DVA Faculty, Newton School of Technology |
| **Institute** | Newton School of Technology |
| **Submission Date** | 28 April 2026 |

### Team Members

| Role | Name | Github |
|---|---|---|
| Project Lead | Praneeth Nakkina Lakshmi | https://github.com/iampraneeth1116 |
| Data Lead | Himank Kaushik | https://github.com/Nobody-KnowsBetter |
| Analysis Lead (EDA) | Priyal Sarda | https://github.com/Priyal-2005 |
| ETL Lead / PPT Lead | Gokul VKS | https://github.com/vks-g |
| Visualization Lead | Atharva Sharma | https://github.com/alpha-sml |
| Report Lead | Abhijeet Sinha | https://github.com/abhijeetnst |

---

## Business Problem

Olist is Brazil's largest e-commerce marketplace, connecting independent sellers to customers through a single contract. Analysis of 99,441 orders placed between September 2016 and August 2018 reveals that 97.19% of customers never placed a second order. This is not a data anomaly — it is a structural failure in post-purchase customer lifecycle management that directly suppresses lifetime value and long-term revenue growth.

This project identifies the measurable behavioural, logistical, and categorical drivers of this single-purchase pattern and translates them into prioritised, actionable retention recommendations with estimated revenue impact.

**Core Business Question**

> Why do 97.19% of Olist customers place exactly one order and never return, and which measurable factors most strongly predict this behaviour?

**Decision Supported**

> Where Olist's Chief Commercial Officer (CCO) and Head of Customer Success should allocate retention investment — whether in promotional programme design, customer segmentation-based outreach, category-specific interventions, or geographic piloting strategy.

---

## Dataset

| Attribute | Details |
|---|---|
| **Source Name** | Brazilian E-Commerce Public Dataset by Olist |
| **Direct Access Link** | https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce |
| **Row Count** | 99,441 orders (raw) → 105,000 rows (stratified processed sample) |
| **Column Count** | 7 raw files · 19 analytical columns in processed master file |
| **Time Period Covered** | September 2016 to August 2018 (23 months) |
| **Format** | CSV (7 relational files) |

**Key Columns Used**

| Column Name | Description | Role in Analysis |
|---|---|---|
| `customer_unique_id` | Canonical real-customer identifier. One real customer can appear under multiple `customer_id` values across orders. Churn is defined at this level — not `customer_id`. | Churn label engineering, all customer-level KPIs |
| `order_purchase_timestamp` | Date and time the customer placed the order | Time-series analysis, days_to_deliver engineering |
| `order_delivered_customer_date` / `order_estimated_delivery_date` | Actual and estimated delivery dates | `delivery_delay_days` computation, logistics KPIs |
| `review_score` | 1–5 customer satisfaction rating (772 orders have no review) | EDA, statistical analysis, Tableau filter dimension |
| `payment_type` | Payment method: credit_card, boleto, voucher, debit_card | Retention segmentation, repeat rate analysis |
| `product_category_name_english` | English translation of product category | Revenue segmentation, category-level churn analysis |
| `churn` | Engineered target: 1 = one order (churned), 0 = two or more orders (retained) | Primary analytical target across all notebooks and dashboard |
| `delivery_delay_days` | Engineered: actual minus estimated delivery in days. Negative = early | Logistics performance KPI, churn driver analysis |
| `order_revenue` | Engineered: price + freight_value | Revenue KPIs, spend segmentation, LTV modelling |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| Overall Churn Rate | % of unique customers who placed exactly one order | `(churned customers / total unique customers) × 100` — NB05 |
| Repeat Customer Rate | % of unique customers who placed 2+ orders | `100 - Churn Rate` — NB05 |
| Total Revenue (R$) | Sum of all order revenue across the dataset | `SUM(order_revenue)` — NB05 |
| Avg Order Value | Mean revenue per order line | `MEAN(order_revenue)` — NB05 |
| On-Time Delivery Rate | % of orders delivered on or before the estimated date | `(delivery_delay_days <= 0).mean() × 100` — NB05 |
| Avg Delivery Delay | Mean delivery_delay_days across all delivered orders | `MEAN(delivery_delay_days)` — NB05 |
| Avg Days to Deliver | Mean actual wait time from order placement to delivery | `MEAN(days_to_deliver)` — NB05 |
| Avg Review Score | Mean review score across all orders with a review | `MEAN(review_score)` — NB05 |
| Voucher Repeat Rate | Repeat rate specifically for customers who paid by voucher | Computed in NB03 and Tableau |
| High-Value Customer % | % of customers in the top spend segment | Computed in NB05 customers_kpi.csv |

Document KPI logic clearly in `notebooks/04_statistical_analysis.ipynb` and `notebooks/05_final_load_prep.ipynb`.

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | _Add Tableau Public link here_ |
| **Executive View** | Page 1 — Customer Retention Insight: Repeat Rate (2.81%), One-time Customers (97.19%), Total Revenue (R$ 14.69M), Customer Lifecycle bar chart, Review Behavior vs Repeat Rate, Payment vs Repeat Rate |
| **Behavioral View** | Page 2 — Customer Behavior Insights: Avg Spend per Customer (R$ 170.84), Avg Orders (1.03), High Value Customer % (0.08%), Spend Distribution, Avg Spend vs Order Count, Spend vs Repeat |
| **Operational View** | Page 3 — Repeat Purchase Barriers and Risk Factors: Overall Churn Rate (97.19%), High Risk Customer % (13.09%), Avg Delivery Delay (10.60 days), Delivery Delay Trend Over Time, Churn by Review Score, Churn Contribution by Customer Value, Churn by Payment Type |
| **Main Filters** | Order Count Bucket, Review Bucket, Payment Type, Spend Segment, Order Month — cross-filtering across all 3 pages |

Dashboard screenshots are stored in [`tableau/screenshots/`](tableau/screenshots/).

---

## Key Insights

1. **97.19% of Olist customers buy once and never return.** This is not category-specific or regional — it is a platform-wide structural failure in post-purchase re-engagement. A lifecycle programme is the only fix.
2. **Delivery performance is strong and is not the primary cause of churn.** 92.9% of orders arrive before the estimated delivery date, with a mean of 12 days ahead of expectation. Improving logistics SLA further will not meaningfully change the churn rate.
3. **Voucher users retain at nearly double the platform average.** Voucher users repeat at 5.86% versus the platform average of 2.81% — the single strongest retention signal in the data. Promotional instruments are already the most effective retention mechanism on the platform.
4. **High-spend customers churn 5.5 percentage points less than low-spend customers.** High-spend segment: 91.78% churn vs 97.26% for low-spend. They represent only 0.08% of the base but generate disproportionate revenue.
5. **Spend compounds with order count.** Average spend per order rises from R$ 141 for 1-order customers to R$ 940 for 4-order customers. Each additional order is worth dramatically more than the last.
6. **Sao Paulo, Rio de Janeiro, and Minas Gerais represent 66.7% of all orders.** These three states are the platform's commercial core and the ideal pilot location for any retention programme.
7. **Northern Brazil faces structural logistics disadvantages.** States like AP, RR, and AM have delivery times of 24–28 days versus a national average of 12 days — a separate retention problem driven by logistics friction.
8. **Health & Beauty and Watches & Gifts are the highest-revenue categories but not natural repeat-purchase categories.** High-consideration purchases with long inter-purchase cycles explain part of the structural churn.
9. **No single variable is a strong independent predictor of churn.** Logistic regression and effect size analysis confirm all tested features have negligible effect sizes (Cohen's d < 0.04, Cramer's V < 0.05). Churn is structural, not feature-driven.
10. **Customers who leave any review — even negative — show marginally higher retention.** The No Review segment has the lowest churn rate (93.75%), suggesting any post-purchase interaction correlates with slightly higher re-engagement likelihood.

---

## Recommendations

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | Voucher users repeat at 5.86% — double the platform average | Deploy automated post-purchase voucher (10–15% discount) to all first-time customers within 7 days of delivery | +1,900 repeat customers → ~R$ 325,000 incremental revenue |
| 2 | High-spend customers churn 5.5 pp less than low-spend | Enrol customers spending R$ 500+ in first order into a VIP loyalty tier with free express shipping and priority support | +200 repeat customers → ~R$ 170,000 |
| 3 | 1–2 star reviews are a leading indicator of disengagement | Auto-trigger personalised apology + R$ 20 credit within 48 hours of a low review submission | +2,400 recovered customers → ~R$ 410,000 |
| 4 | Health & Beauty is top revenue but low natural repeat category | Smart reminder programme 60–90 days post-purchase for consumable categories (Health & Beauty, Bed & Bath, Housewares) | +950 additional orders → ~R$ 132,000 |
| 5 | SP, RJ, MG = 66.7% of all orders with the strongest logistics | Pilot all retention programmes in these 3 states first before national rollout | Statistical validation in 6 weeks · n > 18,000 · unlocks full national scale |

**Combined estimated impact: ~R$ 1.04M in recoverable incremental revenue — approximately 7.1% of total dataset revenue of R$ 14.69M.**

---

## Repository Structure

```text
SectionD_Group-9_Customer_Churn_Analysis/
|
|-- README.md
|
|-- data/
|   |-- raw/                                    # 7 original Olist CSVs (never edited)
|   |   |-- olist_orders_dataset.csv
|   |   |-- olist_customers_dataset.csv
|   |   |-- olist_order_items_dataset.csv
|   |   |-- olist_order_payments_dataset.csv
|   |   |-- olist_order_reviews_dataset.csv
|   |   |-- olist_products_dataset.csv
|   |   `-- product_category_name_translation.csv
|   `-- processed/                              # Cleaned pipeline outputs
|       |-- olist_churn_master.csv              # 105,000 rows × 19 cols — primary analytical file
|       |-- tableau_ready.csv                   # 105,000 rows × 24 cols — Tableau primary source
|       |-- customers_kpi.csv                   # One row per customer — segmentation view
|       |-- monthly_kpi.csv                     # One row per month — trend view
|       `-- state_kpi.csv                       # One row per state — geographic view
|
|-- notebooks/
|   |-- 01_extraction.ipynb                     # Load, inspect, audit all 7 raw files
|   |-- 02_cleaning.ipynb                       # ETL pipeline, merge, feature engineering, churn label
|   |-- 03_eda.ipynb                            # EDA — churn distribution, geo, revenue, delivery, reviews
|   |-- 04_statistical_analysis.ipynb          # T-test, Chi-square, VIF, logistic regression
|   `-- 05_final_load_prep.ipynb               # KPI computation, Tableau export, final verification
|
|-- scripts/
|   `-- etl_pipeline.py                         # Reusable ETL functions: basic_clean, cast_order_datetimes, engineer_delivery_delay
|
|-- logs/
|   |-- 01_extraction_log.md
|   |-- 02_cleaning_log.md
|   |-- 03_eda_log.md
|   |-- 04_statistical_analysis_log.md
|   |-- 05_final_load_prep_log.md
|   `-- 06_tableau_dashboard_design.md
|
|-- tableau/
|   `-- screenshots/
|       |-- Dashboard-1 (Retension).png
|       |-- Dashboard-2 (Behavior).png
|       `-- Dashboard-3 (Churn Drivers).png
|
|-- reports/
|   |-- Report.pdf                              # Final project report (27 pages)
|   `-- Presentation.pdf                        # Final presentation deck
|
|-- docs/
|   `-- data_dictionary.md                      # Full column definitions for olist_churn_master.csv
|
|-- DVA-oriented-Resume/                        # Individual DVA-focused resumes
`-- DVA-oriented-Portfolio/                     # Portfolio case study
```

---

## Analytical Pipeline

The project follows a structured 7-step workflow:

1. **Define** — Sector selected (E-Commerce), problem statement scoped around customer churn, mentor approval obtained.
2. **Extract** — 7 raw Olist CSV files sourced from Kaggle and committed to `data/raw/`; data dictionary drafted in `docs/`.
3. **Clean and Transform** — Cleaning pipeline built in `notebooks/02_cleaning.ipynb` using `scripts/etl_pipeline.py`. Key steps: timestamp parsing, delivered-only filter, 6-table merge, churn label engineering, stratified sample to 105,000 rows.
4. **Analyze** — EDA in NB03 (churn distribution, geo, revenue, delivery, reviews) and statistical analysis in NB04 (T-test, Mann-Whitney U, Cohen's d, Chi-square, Cramer's V, VIF, logistic regression).
5. **Visualize** — Interactive Tableau dashboard built and published on Tableau Public. 3 pages, 5 filter dimensions, 3 dashboard screenshots committed to `tableau/screenshots/`.
6. **Recommend** — 5 data-backed business recommendations delivered, each linked to a quantified insight with estimated revenue impact.
7. **Report** — Final project report (27 pages) and presentation deck completed and exported as PDFs into `reports/`.

---

## Tech Stack

| Tool | Status | Purpose |
|---|---|---|
| Python + Jupyter Notebooks | Mandatory | ETL, cleaning, analysis, and KPI computation |
| Google Colab | Supported | Cloud notebook execution environment |
| Tableau Public | Mandatory | Dashboard design, publishing, and sharing |
| GitHub | Mandatory | Version control, collaboration, contribution audit |

**Python libraries used:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`, `pathlib`

---

## Evaluation Rubric

| Area | Marks | Focus |
|---|---|---|
| Problem Framing | 10 | Is the business question clear and well-scoped? |
| Data Quality and ETL | 15 | Is the cleaning pipeline thorough and documented? |
| Analysis Depth | 25 | Are statistical methods applied correctly with insight? |
| Dashboard and Visualization | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| Business Recommendations | 20 | Are insights actionable and well-reasoned? |
| Storytelling and Clarity | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

> Marks are awarded for analytical thinking and decision relevance, not chart quantity, visual decoration, or code length.

---

## Submission Checklist

**GitHub Repository**

- [x] Public repository created with the correct naming convention (`SectionD_Group-9_Customer_Churn_Analysis`)
- [x] All notebooks committed in `.ipynb` format
- [x] `data/raw/` contains the original, unedited 7 Olist CSV files
- [x] `data/processed/` contains the cleaned pipeline outputs (5 CSV files)
- [x] `tableau/screenshots/` contains all 3 dashboard screenshots
- [x] `tableau/dashboard_links.md` contains the Tableau Public URL
- [x] `docs/data_dictionary.md` is complete
- [x] `README.md` explains the project, dataset, and team
- [x] All members have visible commits and pull requests

**Tableau Dashboard**

- [x] Published on Tableau Public and accessible via public URL
- [x] 5 interactive filters included (Order Count Bucket, Review Bucket, Payment Type, Spend Segment, Order Month)
- [x] Dashboard directly addresses the business problem across 3 pages

**Project Report**

- [x] Final report exported as PDF into `reports/` (Report.pdf — 27 pages)
- [x] Cover page, executive summary, sector context, problem statement
- [x] Data description, cleaning methodology, KPI framework
- [x] EDA with written insights, statistical analysis results
- [x] Dashboard screenshots and explanation
- [x] 10 key insights in decision language
- [x] 5 actionable recommendations with impact estimates
- [x] Contribution matrix matches GitHub history

**Presentation Deck**

- [x] Final presentation exported as PDF into `reports/` (Presentation.pdf)
- [x] Title slide through recommendations, impact, limitations, and next steps

**Individual Assets**

- [x] DVA-oriented resumes committed to `DVA-oriented-Resume/`
- [x] Portfolio case study committed to `DVA-oriented-Portfolio/`

---

## Contribution Matrix

This table must match evidence in GitHub Insights, PR history, and committed files.

| Team Member | Enrollment No. | Dataset & Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT & Viva |
|---|---|---|---|---|---|---|---|---|
| Praneeth Nakkina Lakshmi | 2401010288 | — | Minor | Minor | **Major** | — | — | — |
| Himank Kaushik | 2401010187 | **Major** | — | — | — | — | — | Minor |
| Priyal Sarda | 2401010354 | Minor | — | **Major (EDA)** | — | — | — | — |
| Gokul VKS | 2401020094 | — | **Major** | — | — | Minor | — | **Major** |
| Atharva Sharma | 2401010112 | — | — | — | — | **Major** | Minor | — |
| Abhijeet Sinha | 2401010013 | — | — | — | — | — | **Major** | — |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead:** Praneeth Nakkina Lakshmi &nbsp;&nbsp;&nbsp;&nbsp; **Date:** 28 April 2026

---

## Academic Integrity

All analysis, code, and recommendations in this repository are the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

*Newton School of Technology — Data Visualization & Analytics | Capstone 2*
