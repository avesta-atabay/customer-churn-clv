# Power BI Dashboard

## Main dataset

Import:

```text
data/processed/customer_360_all.csv
```

Supporting datasets:

- `risk_summary_all.csv`
- `action_summary_all.csv`
- `top_20_retention_all.csv`
- `shap_importance.csv`

## Executive Overview

Recommended layout:

1. KPI cards
   - Total Customers
   - Churn Customers
   - Churn Rate
   - Average Churn Probability
   - Expected 90D CLV
   - Revenue at Risk
   - High Risk Customers
   - Critical Risk Customers

2. Risk distribution
   - Donut: `risk_level` × Total Customers

3. Financial risk
   - Column chart: `risk_level` × Revenue at Risk

4. Retention actions
   - Bar chart: `recommended_action` × Total Customers

5. Customer prioritization
   - Scatter: `churn_probability` vs `expected_clv_90d`
   - Size: `revenue_at_risk_90d`
   - Legend: `risk_level`
   - Details: `customer_id`

6. Actionable customer table
   - `customer_id`
   - `churn_probability`
   - `risk_level`
   - `value_segment`
   - `expected_clv_90d`
   - `revenue_at_risk_90d`
   - `recommended_action`

## Data types

| Column | Power BI type / treatment |
|---|---|
| customer_id | Text or Whole Number; use Distinct Count |
| churn_probability | Decimal Number; Percentage formatting |
| return_rate | Decimal Number; Percentage formatting |
| predicted_churn | Whole Number |
| monetary | Decimal Number / Currency |
| avg_order_value | Decimal Number / Currency |
| expected_revenue_90d | Decimal Number / Currency |
| expected_clv_90d | Decimal Number / Currency |
| revenue_at_risk_90d | Decimal Number / Currency |
| retention_priority_score | Decimal Number |
| risk_level | Text |
| rfm_segment | Text |
| value_segment | Text |
| recommended_action | Text |

## Aggregation rule

Do not drag raw numeric columns into KPI cards and accept Power BI's default `Sum` when the metric is a rate/probability.

Use the DAX measures in `dax_measures.dax`.

Especially:

- `churn_probability` → Average
- `return_rate` → Average
- `customer_id` → Distinct Count
- `expected_clv_90d` → Sum
- `revenue_at_risk_90d` → Sum
