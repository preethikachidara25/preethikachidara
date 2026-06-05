<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:0d1117,100:1a1f2e&height=120&text=Customer%20Service%20BI%20Dashboard&fontSize=28&fontColor=00b4d8&desc=Enterprise%20Tableau%20KPI%20Suite%20%C2%B7%20Citibank%20500%2B%20Agents&descColor=90e0ef&descSize=15" />

![Tableau](https://img.shields.io/badge/Tableau%20Certified-0d1117?style=for-the-badge&logo=tableau&logoColor=00b4d8)
![SQL](https://img.shields.io/badge/SQL%20Teradata-0d1117?style=for-the-badge&logo=postgresql&logoColor=90e0ef)
![KPIs](https://img.shields.io/badge/KPIs%20Tracked-8+-brightgreen?style=for-the-badge)
![Impact](https://img.shields.io/badge/Ad--hoc%20Requests%20Reduced-30%25-ff6b6b?style=for-the-badge)

</div>

---

## 📌 Overview

Manual, ad-hoc reporting in large customer service operations is slow and inconsistent. This project documents the **Tableau dashboard suite** I built for Citi's workforce planning team — replacing scattered reports with a centralized BI layer enabling weekly executive reviews across a 500+ agent operation.

---

## 📊 KPIs Tracked

| Category | Metrics |
|---|---|
| **Call Handling** | AHT, First Call Resolution, Queue Wait Time |
| **Escalations** | Escalation Rate, Manual Review Volumes, Team Breakdown |
| **Customer Satisfaction** | CSAT Score, NPS Trends |
| **Workforce** | Agent Utilization, Shrinkage Rate, Shift Adherence |
| **Volume** | Daily/Weekly Inbound, Seasonal Trends, Forecast vs Actual |

---

## 🏗️ Architecture

```
Teradata SQL (optimized — 12% runtime reduction)
        │
        ▼
Tableau Data Extract (.hyper)
        │
        ├──► Executive Summary Dashboard    (senior management)
        ├──► Operational KPI Dashboard      (operations managers)
        ├──► Escalation Analysis Dashboard  (quality team)
        └──► Workforce Utilization View     (workforce planning)
```

---

## 💡 Sample SQL — Rolling Escalation Rate

```sql
SELECT
    team_id,
    week_start_date,
    ROUND(
        100.0 * SUM(escalated_contacts) / NULLIF(SUM(total_contacts), 0), 2
    ) AS escalation_rate_pct,
    AVG(escalation_rate_pct) OVER (
        PARTITION BY team_id
        ORDER BY week_start_date
        ROWS BETWEEN 3 PRECEDING AND CURRENT ROW
    ) AS rolling_4wk_avg
FROM contact_summary
GROUP BY team_id, week_start_date
ORDER BY team_id, week_start_date;
```

---

## 🗂️ Repository Structure

```
customer-service-bi-dashboard/
├── sql/
│   ├── kpi_escalation_rate.sql
│   ├── kpi_handle_time.sql
│   ├── kpi_csat_trends.sql
│   └── workforce_utilization.sql
├── tableau/
│   └── dashboard_mockup_notes.md
├── docs/
│   └── kpi_definitions.md
└── README.md
```

---

## 🛠️ Tools & Libraries

![Tableau](https://img.shields.io/badge/Tableau-0d1117?style=flat-square&logo=tableau&logoColor=00b4d8)
![SQL](https://img.shields.io/badge/SQL%20Teradata-0d1117?style=flat-square&logo=postgresql&logoColor=90e0ef)
![Python](https://img.shields.io/badge/Python-0d1117?style=flat-square&logo=python&logoColor=cae9ff)
![Excel](https://img.shields.io/badge/Excel-0d1117?style=flat-square&logo=microsoftexcel&logoColor=48cae4)

---

> *Dashboards contain confidential operational data and are not publicly shareable. Repository documents methodology, SQL patterns, and design decisions.*

<div align="center">
<sub>Part of <a href="https://github.com/preethikachidara">Preethika Chidara's</a> analytics portfolio</sub>
</div>
