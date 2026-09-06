# Project Notes

## Current analytical snapshot

The cleaned transaction pipeline produced 779,425 valid sales transactions and 18,390 cancellation/return transactions from 1,067,371 raw rows.

The customer feature dataset used for churn modeling contains 5,281 customers. The temporal churn label is 56.6% positive.

## Final model

Random Forest was selected as the primary model based on ROC-AUC / PR-AUC and its strong balance between precision and recall.

A validation-based threshold of 0.40 was selected for the final test evaluation.

Final test metrics:

- Accuracy: 0.7427
- Precision: 0.7277
- Recall: 0.8712
- F1: 0.7930
- ROC-AUC: 0.8114
- PR-AUC: 0.8250

## Business interpretation

The model is intended for prioritization, not as an autonomous decision-maker. High predicted churn probability should be combined with customer value and expected financial impact before allocating retention budget.
