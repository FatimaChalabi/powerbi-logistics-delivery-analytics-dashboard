# 🚚 Power BI Logistics & Delivery Analytics Dashboard

Interactive Power BI dashboard for logistics & delivery analytics — tracking on-time performance, driver efficiency, SLA breaches, and profit margins with a live fuel-cost simulator, across 6 regional hubs and 45K deliveries.

![Status](https://img.shields.io/badge/status-complete-brightgreen) ![Power BI](https://img.shields.io/badge/tool-Power%20BI-F2C811) ![DAX](https://img.shields.io/badge/DAX-custom%20measures-0B5D5D)

---

## 📊 Overview

This project simulates a regional logistics company operating across 6 hubs, tracking every delivery from dispatch to completion — including on-time performance, SLA breaches, driver/vehicle efficiency, customer segments, and a fuel-cost what-if simulator for profitability scenarios.

**Key highlights:**
- Star-schema data model (1 fact table + 5 dimension tables)
- Custom DAX measures for SLA tracking, YoY growth, and cost-per-km analysis
- Interactive **Fuel Price Simulator** (What-If parameter) for margin scenario planning
- Driver performance leaderboard with experience-vs-efficiency analysis
- Custom report theme applied consistently across all pages

---

## 🗂️ Dashboard Pages

| # | Page | What it answers |
|---|------|------------------|
| 1 | **Executive Overview** | What's our overall delivery volume, revenue, and on-time rate? |
| 2 | **Delivery Performance** | Which hubs/vehicle types are underperforming on SLA? |
| 3 | **Driver & Fleet Analysis** | Who are our top drivers, and does experience correlate with performance? |
| 4 | **Customer & SLA Insights** | Which customer segments have the highest failure/breach rates? |
| 5 | **Profitability & Simulation** | How does a fuel price change impact our margin, by vehicle type? |

---

## 🧱 Data Model

```
dim_date ────┐
dim_hub ─────┤
dim_vehicle ─┼──► fact_deliveries (45,000 rows)
dim_driver ──┤
dim_customer ┘
```

- **fact_deliveries** — one row per delivery: distance, weight, promised vs. actual time, status, cost, revenue, rating
- **dim_date** — marked as Date Table for time intelligence
- **dim_hub** — 6 regional hubs across Azerbaijan
- **dim_vehicle** — 67 vehicles (Motosiklet / Kiçik Furqon / Furqon / Yük Maşını)
- **dim_driver** — 88 drivers with experience & rating
- **dim_customer** — 1,500 customers across 4 segments
