# 📊 Customer Service BI Dashboard Suite

> **Enterprise Tableau dashboards tracking 8+ KPIs for Citi's 500+ agent customer service operation**

## Overview

Manual reporting in large customer service operations is slow, inconsistent, and costly to maintain. This project documents the Tableau dashboard suite I built for Citi's workforce planning and customer service team — replacing ad-hoc reporting with a standardized, interactive BI layer that enabled weekly operational reviews for senior management.

---

## Business Context

- **Organization:** Citibank Customer Service Department (500+ agents)
- **Users:** Operations managers, workforce planning leads, senior management
- **Reporting cadence:** Weekly operational reviews + real-time monitoring
- **Problem solved:** Eliminated ~30% of ad-hoc reporting requests by centralizing KPIs into self-serve dashboards

---

## KPIs Tracked

| Category | Metrics |
|---|---|
| **Call Handling** | Average Handle Time (AHT), First Call Resolution (FCR), Queue Wait Time |
| **Escalations** | Escalation Rate, Manual Review Volumes, Escalation by Team/Region |
| **Customer Satisfaction** | CSAT Score, Net Promoter Score (NPS) trends |
| **Workforce** | Agent Utilization, Shrinkage Rate, Shift Adherence |
| **Volume** | Daily/Weekly Inbound Volume, Seasonal Trends, Forecast vs Actual |

---

## Dashboard Architecture

```
Data Source (Teradata SQL)
        │
        ▼ (Optimized SQL extracts — ~12% runtime reduction)
Tableau Data Extract (.hyper)
        │
        ├──► Executive Summary Dashboard   (senior management)
        ├──► Operational KPI Dashboard     (operations managers)
        ├──► Escalation Analysis Dashboard (quality team)
        └──► Workforce Utilization View    (workforce planning)
```

---

## SQL Optimization Highlights

Reporting pipelines on Teradata were optimized to reduce query runtime by ~12%:

- Replaced correlated subqueries with window functions (`ROW_NUMBER`, `RANK`)
- Moved heavy aggregations to intermediate derived tables
- Indexed frequently filtered columns (agent_id, date_key, team_id)
- Scheduled incremental extracts instead of full refreshes

```sql
-- Example: Optimized escalation rate by team (window function approach)
SELECT
    team_id,
    week_start_date,
    SUM(escalated_contacts) AS escalations,
    SUM(total_contacts) AS total_contacts,
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

## Repository Structure

```
customer-service-bi-dashboard/
│
├── sql/
│   ├── kpi_escalation_rate.sql
│   ├── kpi_handle_time.sql
│   ├── kpi_csat_trends.sql
│   └── workforce_utilization.sql
│
├── tableau/
│   └── dashboard_mockup_notes.md    # Layout and design decisions
│
├── docs/
│   └── kpi_definitions.md           # Business definitions for each metric
│
└── README.md
```

---

## Tools & Stack

`Tableau Desktop` `Tableau Server` `SQL (Teradata)` `Excel` `Python (data validation)`

---

## Design Principles

- **Audience-first:** Each dashboard tailored to its user — executives see summary, managers see drill-down
- **Single source of truth:** All KPIs computed from the same SQL layer to avoid metric inconsistencies
- **Action-oriented:** Every view designed to answer "what do I do next?" not just "what happened?"

---

> *Dashboards contain confidential operational data and are not publicly shareable. This repository documents methodology, SQL patterns, and design decisions.*
