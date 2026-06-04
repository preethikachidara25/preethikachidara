# 📈 Staffing Demand Forecasting Tool

> **Python-based 6-month workforce demand forecasting — prevented ~$100K/month in contractor over-allocation losses at Citibank**

## Overview

Workforce over-staffing and under-staffing in large customer service operations is a costly problem. At Citi's customer service department (500+ agents), inaccurate contractor allocation was leading to significant monthly financial exposure.

This project documents the forecasting framework I built to solve that problem — a Python tool that predicted 6-month staffing demand with **<5% forecasting error**, enabling optimized contractor allocation and eliminating unnecessary operational payouts.

---

## Business Problem

| Metric | Before | After |
|---|---|---|
| Forecasting method | Manual estimation | Python time series model |
| Forecasting horizon | 4–6 weeks | 6 months |
| Forecasting error | ~15–20% | < 5% |
| Monthly financial risk | ~$100K exposure | Mitigated |

---

## Approach

```
Historical Staffing Data
        │
        ▼
Feature Engineering
(Seasonality · Business Volume Drivers · Call Volume Trends · Leave Patterns)
        │
        ▼
Time Series Forecasting Model
        │
        ├──► 6-Month Demand Prediction per Team/Shift
        │
        └──► Contractor Allocation Recommendations
                    │
                    └──► Workforce Planning Dashboard (Tableau)
```

### Key Modeling Decisions
- Incorporated **seasonality factors** (holiday periods, product launch cycles)
- Used **business volume drivers** (transaction volumes, escalation rate trends) as leading indicators
- Applied rolling validation to ensure <5% error threshold on holdout periods
- Output structured for direct use by operations managers in weekly planning meetings

---

## Repository Structure

```
staffing-demand-forecasting/
│
├── data/
│   └── sample_data_schema.md        # Schema description (no PII/confidential data)
│
├── notebooks/
│   ├── 01_exploratory_analysis.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_forecasting_model.ipynb
│
├── src/
│   ├── data_preprocessing.py
│   ├── feature_engineering.py
│   ├── forecasting_model.py
│   └── allocation_optimizer.py
│
├── outputs/
│   └── forecast_report_template.xlsx
│
└── README.md
```

---

## Technical Stack

`Python` `Pandas` `NumPy` `Scikit-learn` `Statsmodels` `Matplotlib` `Seaborn` `SQL (Teradata)`

---

## Key Takeaways

- **Seasonality matters more than raw volume** — incorporating leave patterns and business cycles reduced error by ~40% compared to naive volume-based models
- **Stakeholder trust requires explainability** — weekly output was designed to be readable by non-technical operations managers, not just data teams
- **Validation strategy** — rolling 4-week holdout windows prevented data leakage and ensured the <5% error held on truly unseen periods

---

> *Note: All data used in this project was internal to Citibank and is not included in this repository. Code reflects the general framework and methodology.*
