# Customer Churn & CLV Analytics Platform

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/) [![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/) [![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

An end-to-end e-commerce analytics project that connects **data cleaning → customer segmentation → churn prediction → explainability → customer value → retention actions → Power BI**.

> **Portfolio focus:** this project is designed to demonstrate that I can turn transaction data into a predictive model and then translate model output into business decisions.

## Business Problem

Which customers are likely to churn, how valuable are those customers, and **what should the business do before losing them?**

The platform produces a customer-level **Customer 360** dataset containing:

- RFM metrics and customer segments
- churn probability and predicted churn
- risk level
- customer value segment
- expected 90-day orders and revenue
- expected 90-day customer value
- revenue at risk
- retention priority score
- recommended retention action

## Tech Stack

| Area | Tools |
|---|---|
| Data preparation | Python, Pandas, NumPy |
| EDA / visualization | Matplotlib, Seaborn |
| Customer analytics | RFM, feature engineering |
| Machine learning | Scikit-learn, Random Forest, XGBoost |
| Explainability | SHAP |
| BI | Power BI, DAX |
| Data format | Excel, CSV |

## Project Pipeline

```text
Online Retail II
      │
      ▼
Data Cleaning
      │
      ├── invalid prices
      ├── missing customers/dates
      └── cancellations / returns
      │
      ▼
Customer Feature Engineering
      │
      ├── Recency
      ├── Frequency
      ├── Monetary
      ├── Total Items
      ├── Unique Products
      ├── Average Order Value
      └── Customer Tenure
      │
      ▼
RFM Segmentation
      │
      ▼
Temporal Churn Label
      │
      ▼
Model Comparison
 ┌───────────────┐
 │ Logistic Reg. │
 │ Random Forest │
 │ XGBoost       │
 └───────────────┘
      │
      ▼
Threshold Optimization
      │
      ▼
Final Random Forest
      │
      ├── SHAP Explainability
      │
      ▼
Customer 360 Scoring
      │
      ▼
90-Day Customer Value
      │
      ├── Expected Revenue
      ├── Expected CLV
      └── Revenue at Risk
      │
      ▼
Retention Action Engine
      │
      ▼
Power BI Executive Dashboard
```

## Dataset

This project uses the **Online Retail II** transaction dataset. The raw dataset is intentionally **not committed to GitHub**. Place the downloaded workbook here:

```text
data/raw/online_retail_II.xlsx
```

Expected core columns:

```text
Invoice
StockCode
Description
Quantity
InvoiceDate
Price
Customer ID
Country
```

The notebook standardizes these into:

```text
invoice_no
stockcode
description
quantity
invoice_date
unit_price
customer_id
country
```

## Repository Structure

```text
customer-churn-clv/
│
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
│
├── notebooks/
│   └── customer_churn_clv_analysis.ipynb
│
├── src/
│   └── README.md
│
├── data/
│   ├── raw/
│   │   └── .gitkeep
│   └── processed/
│       └── .gitkeep
│
├── powerbi/
│   ├── README.md
│   └── dax_measures.dax
│
└── docs/
    └── project_notes.md
```

## Reproduced Results

The current analysis produced the following key results before Power BI:

- Raw transactions: **1,067,371**
- Valid sales transactions after cleaning: **779,425**
- Cancellation/return transactions: **18,390**
- Unique sales customers: **5,878**
- Model dataset: **5,281 customers**
- Churn label distribution: **56.6% churn / 43.4% non-churn**

### Model comparison

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Random Forest | 0.760 | 0.788 | 0.788 | 0.788 | **0.811** | **0.825** |
| XGBoost | 0.754 | 0.751 | **0.844** | **0.795** | 0.806 | 0.823 |
| Logistic Regression | 0.744 | 0.757 | 0.804 | 0.780 | 0.801 | 0.824 |

For the final Random Forest, validation-based threshold selection chose **0.40**. On the held-out test set:

- Accuracy: **0.743**
- Precision: **0.728**
- Recall: **0.871**
- F1: **0.793**
- ROC-AUC: **0.811**
- PR-AUC: **0.825**

The threshold is intentionally selected using the validation set rather than the test set.

## Explainability

SHAP analysis showed the strongest model drivers were:

1. **Recency**
2. **Frequency**
3. **Total Items**
4. **Monetary**
5. **Unique Products**
6. Customer Tenure
7. Average Order Value
8. Return Rate
9. Return Transactions

This supports an interpretable business narrative: **recent customer activity is the strongest signal in the current model**, while purchasing depth and historical value provide additional context.

## Retention Strategy

The project does not stop at a probability score. Customers are mapped to actions such as:

- VIP Human Retention
- Personalized Retention Offer
- Automated Win-Back Campaign
- VIP Loyalty & Upsell
- Standard Engagement
- Monitor

The action engine combines **churn probability + customer value** so that the business can prioritize customers where retention has the greatest expected financial impact.

## Power BI

The main Power BI source is:

```text
data/processed/customer_360_all.csv
```

Supporting exports include:

```text
risk_summary_all.csv
action_summary_all.csv
top_20_retention_all.csv
shap_importance.csv
```

See [`powerbi/README.md`](powerbi/README.md) for the dashboard specification and DAX measures.

## How to Run

### 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd customer-churn-clv
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv venv
venv\\Scripts\\activate
```

macOS/Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Put the Excel file at:

```text
data/raw/online_retail_II.xlsx
```

### 5. Run the notebook

```bash
jupyter notebook
```

Open:

```text
notebooks/customer_churn_clv_analysis.ipynb
```

Run the cells from top to bottom.

### 6. Load the exported CSV into Power BI

Use:

```text
data/processed/customer_360_all.csv
```

Then create the measures from:

```text
powerbi/dax_measures.dax
```

## Important Modeling Note

The project uses a **temporal churn definition**: customer activity is measured before a cutoff date and churn is determined from inactivity during the following prediction window. This is preferable to randomly labeling customers because it better represents a real forecasting scenario.

`expected_clv_90d` is a **90-day expected customer value proxy based on observed purchase rate, average order value, and churn probability**. It is not a full probabilistic lifetime-value model such as BG/NBD + Gamma-Gamma.

## Future Improvements

- Time-based cross-validation
- Probability calibration
- Hyperparameter optimization with Optuna
- BG/NBD + Gamma-Gamma CLV
- Uplift modeling for retention treatment selection
- Cost-sensitive retention optimization
- Power BI drill-through customer profile
- Automated model monitoring

## License

This repository contains the project code and documentation. The source dataset remains external and is not redistributed by this repository.
