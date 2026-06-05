<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:0d1117,100:1a1f2e&height=120&text=Staffing%20Demand%20Forecasting&fontSize=30&fontColor=00b4d8&desc=Prevented%20~%24100K%2Fmonth%20in%20contractor%20over-allocation%20losses%20%C2%B7%20Citibank&descColor=90e0ef&descSize=14" />

![Python](https://img.shields.io/badge/Python-3.8+-00b4d8?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-0d1117?style=for-the-badge&logo=pandas&logoColor=90e0ef)
![Forecast Error](https://img.shields.io/badge/Forecast%20Error-%3C5%25-brightgreen?style=for-the-badge)
![Horizon](https://img.shields.io/badge/Horizon-6%20Months-00b4d8?style=for-the-badge)
![Impact](https://img.shields.io/badge/Impact-~%24100K%2Fmonth%20Saved-ff6b6b?style=for-the-badge)

</div>

---

## 📌 Overview

Workforce over-staffing in large customer service operations is a costly, often invisible problem. At Citi's 500+ agent customer service department, manual estimation was creating significant monthly financial exposure through contractor over-allocation.

This project documents the **Python-based 6-month staffing demand forecasting tool** I built to solve that — achieving **<5% forecasting error** and eliminating ~$100K/month in unnecessary operational payouts.

---

## 📊 Business Impact

<table>
<tr>
<td align="center" width="200"><b>Before</b></td>
<td align="center" width="200"><b>After</b></td>
</tr>
<tr>
<td>Manual estimation</td>
<td>Python time series model</td>
</tr>
<tr>
<td>4–6 week horizon</td>
<td><b>6-month horizon</b></td>
</tr>
<tr>
<td>~15–20% error</td>
<td><b>&lt;5% error</b></td>
</tr>
<tr>
<td>~$100K monthly risk</td>
<td><b>Risk mitigated</b></td>
</tr>
</table>

---

## ⚙️ Architecture

```
Historical Staffing Data
        │
        ▼
Feature Engineering
├── Seasonality (holidays, product cycles)
├── Business Volume Drivers (transaction vol, call trends)
├── Leave Patterns (PTO, sick leave history)
└── Escalation Rate Trends
        │
        ▼
Time Series Forecasting Model
        │
        ├──► 6-Month Demand Prediction (per team/shift)
        └──► Contractor Allocation Recommendations
                    │
                    └──► Workforce Planning Dashboard (Tableau)
```

---

## 🗂️ Repository Structure

```
staffing-demand-forecasting/
├── data/
│   └── sample_data_schema.md
├── notebooks/
│   ├── 01_exploratory_analysis.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_forecasting_model.ipynb
├── src/
│   ├── data_preprocessing.py
│   ├── feature_engineering.py
│   ├── forecasting_model.py
│   └── allocation_optimizer.py
├── outputs/
│   └── forecast_report_template.xlsx
└── README.md
```

---

## 🛠️ Tools & Libraries

![Python](https://img.shields.io/badge/Python-0d1117?style=flat-square&logo=python&logoColor=00b4d8)
![Pandas](https://img.shields.io/badge/Pandas-0d1117?style=flat-square&logo=pandas&logoColor=90e0ef)
![NumPy](https://img.shields.io/badge/NumPy-0d1117?style=flat-square&logo=numpy&logoColor=cae9ff)
![Sklearn](https://img.shields.io/badge/Scikit--learn-0d1117?style=flat-square&logo=scikitlearn&logoColor=48cae4)
![SQL](https://img.shields.io/badge/SQL%20(Teradata)-0d1117?style=flat-square&logo=postgresql&logoColor=00b4d8)
`Statsmodels` `Matplotlib` `Seaborn`

---

> *All data used is internal to Citibank and not included in this repository. Code reflects the general framework and methodology.*

<div align="center">
<sub>Part of <a href="https://github.com/preethikachidara">Preethika Chidara's</a> analytics portfolio</sub>
</div>
