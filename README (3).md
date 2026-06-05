<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:0d1117,100:1a1f2e&height=120&text=Freight%20Analytics%20Data%20Warehouse&fontSize=26&fontColor=00b4d8&desc=Star-Schema%20DW%20%2B%20Driver%20KPI%20Framework%20%C2%B7%20Groendyke%20Transport&descColor=90e0ef&descSize=14" />

![SQL](https://img.shields.io/badge/SQL-0d1117?style=for-the-badge&logo=postgresql&logoColor=00b4d8)
![Python](https://img.shields.io/badge/Python-0d1117?style=for-the-badge&logo=python&logoColor=90e0ef)
![Datasets](https://img.shields.io/badge/Datasets%20Integrated-5+-brightgreen?style=for-the-badge)
![Schema](https://img.shields.io/badge/Schema-Star%20Schema-00b4d8?style=for-the-badge)

</div>

---

## 📌 Overview

Groendyke Transport needed a data-driven foundation to evaluate driver performance — but operational data lived in siloed systems with no unified analytical layer. This project documents the **end-to-end data warehouse** I designed integrating 5+ sources, plus a KPI framework that separates driver skill from external factors like weather and traffic.

---

## 🏗️ Architecture

```
Source Systems
├── Trip Logs          (completion times, routes, loads)
├── Weather Data       (conditions per route/date)
├── Traffic Data       (congestion scores by corridor)
├── Driver Schedules   (shift patterns, hours)
└── Payroll/Wage Data
        │
        ▼
ETL Pipeline (Python + SQL)
        │
        ▼
Data Warehouse — Star Schema
├── fact_trip                      ← Core fact table
└── Dimensions
    ├── dim_driver
    ├── dim_route
    ├── dim_date
    └── dim_conditions
        │
        ▼
KPI Layer (SQL Views)
        └──► Driver Performance Dashboard
```

---

## 📐 Data Model

### `fact_trip` — Core Fact Table

| Column | Type | Description |
|---|---|---|
| trip_id | INT | Primary key |
| driver_id | INT | FK → dim_driver |
| route_id | INT | FK → dim_route |
| date_key | INT | FK → dim_date |
| condition_key | INT | FK → dim_conditions |
| planned_duration_min | FLOAT | Scheduled trip time |
| actual_duration_min | FLOAT | Actual completion time |
| delay_min | FLOAT | Actual minus planned |
| completion_status | VARCHAR | On-time / Delayed / Early |

---

## 📊 KPI Framework

| KPI | Purpose |
|---|---|
| **On-Time Rate** | Core performance metric |
| **Adjusted Delay Score** | Isolates driver contribution from weather/traffic |
| **Load Efficiency** | Throughput measure |
| **Schedule Adherence** | Compliance metric |
| **Route Consistency** | Driver reliability signal |

---

## 💡 Sample SQL — Adjusted Driver Performance

```sql
WITH trip_baseline AS (
    SELECT route_id, weather_category, traffic_score,
           AVG(actual_duration_min) AS avg_duration_baseline
    FROM fact_trip f
    JOIN dim_route r ON f.route_id = r.route_id
    JOIN dim_conditions c ON f.condition_key = c.condition_key
    GROUP BY route_id, weather_category, traffic_score
)
SELECT
    d.driver_id, d.driver_name,
    COUNT(*) AS total_trips,
    ROUND(AVG(f.actual_duration_min - b.avg_duration_baseline), 2) AS avg_adjusted_delay_min,
    ROUND(100.0 * SUM(CASE WHEN f.actual_duration_min <= b.avg_duration_baseline THEN 1 ELSE 0 END)
          / COUNT(*), 1) AS adjusted_ontime_pct
FROM fact_trip f
JOIN dim_driver d ON f.driver_id = d.driver_id
JOIN dim_route r ON f.route_id = r.route_id
JOIN dim_conditions c ON f.condition_key = c.condition_key
JOIN trip_baseline b ON b.route_id = r.route_id
    AND b.weather_category = c.weather_category
    AND b.traffic_score = c.traffic_score
GROUP BY d.driver_id, d.driver_name
ORDER BY avg_adjusted_delay_min ASC;
```

---

## 🗂️ Repository Structure

```
freight-analytics-data-warehouse/
├── sql/
│   ├── ddl/         ← Table creation scripts
│   ├── etl/         ← Load scripts
│   └── kpis/        ← KPI view definitions
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   └── 02_kpi_analysis.ipynb
├── docs/
│   └── data_dictionary.md
└── README.md
```

---

## 🛠️ Tools & Libraries

![SQL](https://img.shields.io/badge/SQL-0d1117?style=flat-square&logo=postgresql&logoColor=00b4d8)
![Python](https://img.shields.io/badge/Python-0d1117?style=flat-square&logo=python&logoColor=90e0ef)
![Pandas](https://img.shields.io/badge/Pandas-0d1117?style=flat-square&logo=pandas&logoColor=cae9ff)
![Matplotlib](https://img.shields.io/badge/Matplotlib-0d1117?style=flat-square&logo=plotly&logoColor=48cae4)
`Star Schema Design` `ETL Pipelines` `Data Modeling`

---

> *Client data is confidential and not included. Repository contains schema design, KPI logic, and sample SQL.*

<div align="center">
<sub>Part of <a href="https://github.com/preethikachidara">Preethika Chidara's</a> analytics portfolio</sub>
</div>
