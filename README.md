# Olist E-commerce Pipeline & Dashboard

End-to-end data pipeline (Databricks, Medallion Architecture) and Tableau dashboard on the Olist Brazilian E-commerce dataset. Business question: how do sales and delivery service vary across time, product categories and customer locations, and what should the business investigate?

🚧 **Status:** Completed. The pipeline is complete and the dashboards have been been finalized.

@Christine: Please see version 1.1B of our runbook to replicate our steps in your own Databricks account :)

## Team 3 "The Movie Magic Team" credits

Team project by four members of the Generation Singapore Junior Data Engineer program (Amelia, Clarence, Fauzi, Riani). The pipeline follows our shared team guide (Databricks notebook, Bronze → Silver → Gold), and every member ran it end-to-end in their own workspace. Gold layer tables were exported and processed in Tableau Public and PowerBI Desktop.

## Tech stack

Databricks (Python/PySpark, SQL), Tableau Public, (Power BI Desktop)

## Data flow

Kaggle CSVs (9 files) → Bronze (raw) → Silver (cleaned, validated, audit tables) → Gold (dimension & fact tables) → CSV extracts (orders, items) → Tableau (& Power BI)

## Baseline metrics

| Metric           | Value    |
| ---------------- | -------- |
| Total orders     | 99,441   |
| Delivered GMV    | R$13.22M |
| AOV              | R$137    |
| On-time delivery | 93.22%   |
| Avg review score | 4.09     |

## Data quality & validation

- Duplicate removal, blank-to-null conversion and type casting, with an audit table for cast errors
- Primary-key duplicate and NULL checks that block the release when they fail
- City name cleaning (Unicode, accents, spacing, case) with an audit trail
- Reconciliation of item value against payment totals
- Unit tests for business rules (money joins, on-time delivery)
- Idempotent writes (`mode('overwrite')`), so re-runs are safe
- Control totals compared between Databricks and Tableau

## Key findings

**1. Orders are concentrated in São Paulo.**
São Paulo has about 42% of all orders (41,746 of 99,441), followed by Rio de Janeiro (13%) and Minas Gerais (12%).

**2. Delivery is slowest in Brazil's North and Northeast, but these states are a small share of orders.**
The 10 slowest states are all in the North or Northeast and together account for about 4.7% of orders (about 4,700 of 99,441). The three slowest have small samples (Roraima 29.3 days on 46 orders, Amapá 68 orders, Amazonas 148 orders), so their averages should be read with caution. Higher-volume states such as Ceará (1,336 orders), Pará (975) and Maranhão (747) also average 21.2 to 23.8 days, so the pattern is not only small-sample noise.

## Metric definitions

- **GMV (delivered):** sum of item prices of delivered orders, excluding freight. Not profit.
- **AOV:** delivered GMV ÷ number of delivered orders.
- **On-time rate:** share of eligible delivered orders delivered on or before the estimated date.

## Limitations

Historical data (2016-2018). September-October 2018 are excluded from the trend chart because they are incomplete (16 and 4 orders); As such, **our Dashboards are cutoff at 31 August 2018***.  KPI totals include all orders. Late 2016 also has very few orders.

## Dataset

Olist Brazilian E-Commerce Public Dataset, provided by Olist and hosted on [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Raw CSVs are not included in this repository.
