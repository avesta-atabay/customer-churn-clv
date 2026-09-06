# Source Code

The current portfolio version keeps the primary reproducible workflow in the Jupyter notebook under `notebooks/` so that the complete analytical process can be inspected cell-by-cell.

A future refactor can split the notebook into modules such as:

```text
src/
├── data_cleaning.py
├── features.py
├── rfm.py
├── churn_model.py
├── clv.py
└── retention.py
```
