# 🚛 Freight Analytics Data Warehouse — Groendyke Transport

> **End-to-end data warehouse design + KPI framework for driver performance evaluation at a freight & cargo company**

## Overview

Groendyke Transport needed a data-driven foundation to evaluate driver performance and freight efficiency — but their operational data lived in siloed systems with no unified analytical layer. This project documents the data warehouse I designed and built as part of a client analytics engagement, integrating 5+ operational datasets and delivering a KPI framework to support data-driven driver wage evaluation.

---

## Business Problem

- **No unified data layer** — trip logs, weather feeds, traffic data, and driving schedules were siloed
- **Subjective driver evaluation** — performance reviews lacked quantitative grounding
- **Inefficiency blind spots** — no visibility into where delays or inefficiencies occurred across routes

---

## Solution Architecture

```
Source Systems
│
├── Trip Logs (completion times, routes, loads)
├── Weather Data (conditions per route/date)
├── Traffic Data (congestion scores by corridor)
├── Driver Schedules (shift patterns, hours)
└── Payroll/Wage Data
        │
        ▼
ETL Pipeline (Python + SQL)
        │
        ▼
Data Warehouse (Star Schema)
        │
        ├── Fact Table: fact_trip
        └── Dimension Tables:
                ├── dim_driver
                ├── dim_route
                ├── dim_date
                └── dim_conditions (weather + traffic)
        │
        ▼
KPI Layer (SQL Views)
        │
        └── Driver Performance Dashboard
```

---

## Data Model

### Fact Table: `fact_trip`
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
| load_weight_lbs | FLOAT | Cargo weight |
| completion_status | VARCHAR | On-time / Delayed / Early |

### Key Dimension Tables
- **dim_driver** — driver_id, name, tenure, region, shift_type
- **dim_route** — route_id, origin, destination, distance_miles, road_type
- **dim_conditions** — condition_key, weather_category, traffic_score, temp_range

---

## KPI Framework

KPIs were designed to isolate driver skill from external factors (weather, traffic):

| KPI | Formula | Purpose |
|---|---|---|
| **On-Time Rate** | On-time trips / Total trips | Core performance metric |
| **Adjusted Delay Score** | Avg delay controlling for weather + traffic | Isolates driver contribution |
| **Load Efficiency** | Avg load weight × completion rate | Throughput measure |
| **Schedule Adherence** | Trips within shift window / Total trips | Compliance metric |
| **Route Consistency** | Std dev of completion time by route | Driver reliability signal |

---

## Sample SQL — Adjusted Driver Performance Score

```sql
-- Compute driver performance adjusted for external conditions
WITH trip_baseline AS (
    SELECT
        r.route_id,
        c.weather_category,
        c.traffic_score,
        AVG(f.actual_duration_min) AS avg_duration_baseline
    FROM fact_trip f
    JOIN dim_route r ON f.route_id = r.route_id
    JOIN dim_conditions c ON f.condition_key = c.condition_key
    GROUP BY r.route_id, c.weather_category, c.traffic_score
),
driver_vs_baseline AS (
    SELECT
        f.driver_id,
        f.trip_id,
        f.actual_duration_min,
        b.avg_duration_baseline,
        f.actual_duration_min - b.avg_duration_baseline AS adjusted_delta
    FROM fact_trip f
    JOIN dim_route r ON f.route_id = r.route_id
    JOIN dim_conditions c ON f.condition_key = c.condition_key
    JOIN trip_baseline b
        ON b.route_id = r.route_id
        AND b.weather_category = c.weather_category
        AND b.traffic_score = c.traffic_score
)
SELECT
    d.driver_id,
    d.driver_name,
    COUNT(*) AS total_trips,
    ROUND(AVG(dvb.adjusted_delta), 2) AS avg_adjusted_delay_min,
    ROUND(100.0 * SUM(CASE WHEN dvb.adjusted_delta <= 0 THEN 1 ELSE 0 END) / COUNT(*), 1) AS adjusted_ontime_pct
FROM driver_vs_baseline dvb
JOIN dim_driver d ON dvb.driver_id = d.driver_id
GROUP BY d.driver_id, d.driver_name
ORDER BY avg_adjusted_delay_min ASC;
```

---

## Repository Structure

```
freight-analytics-data-warehouse/
│
├── sql/
│   ├── ddl/
│   │   ├── create_fact_trip.sql
│   │   ├── create_dim_driver.sql
│   │   ├── create_dim_route.sql
│   │   └── create_dim_conditions.sql
│   ├── etl/
│   │   └── load_fact_trip.sql
│   └── kpis/
│       ├── driver_ontime_rate.sql
│       ├── adjusted_delay_score.sql
│       └── route_efficiency.sql
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   └── 02_kpi_analysis.ipynb
│
├── docs/
│   └── data_dictionary.md
│
└── README.md
```

---

## Tools & Stack

`Python` `SQL` `Pandas` `Matplotlib` `Star Schema Design` `ETL`

---

## Key Outcomes

- Identified **3 key efficiency improvement opportunities** across freight operations
- KPI framework adopted for quarterly driver performance reviews
- Adjusted delay score revealed that ~40% of apparent delays were attributable to weather/traffic, not driver performance — leading to fairer wage evaluation criteria

---

> *Client data is confidential and not included. This repository contains schema design, KPI logic, and sample SQL.*
