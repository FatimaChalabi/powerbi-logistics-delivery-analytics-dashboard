# DAX Measures Reference

All measures live in the `_Measures` table. Grouped by the page where they're first used.

---

## Core / Executive Overview

```dax
Total Deliveries = COUNTROWS(fact_deliveries)

Total Revenue = SUM(fact_deliveries[Revenue])

Total Cost = SUM(fact_deliveries[DeliveryCost]) + SUM(fact_deliveries[FuelCost])

Total Profit = [Total Revenue] - [Total Cost]

Profit Margin % = DIVIDE([Total Profit], [Total Revenue])

On-Time Deliveries = CALCULATE([Total Deliveries], fact_deliveries[Status] = "Vaxtında")

On-Time Delivery % = DIVIDE([On-Time Deliveries], [Total Deliveries])
```

### Revenue YoY %

Uses a trailing 12-month comparison instead of `SAMEPERIODLASTYEAR`, so it returns a meaningful
result even on pages with no date slicer (like Executive Overview, where all years are visible
at once and a calendar-year YoY would be undefined).

```dax
Revenue YoY % =
VAR Last12M = CALCULATE([Total Revenue], DATESINPERIOD(dim_date[Date], MAX(dim_date[Date]), -12, MONTH))
VAR Prior12M = CALCULATE([Total Revenue], DATESINPERIOD(dim_date[Date], EDATE(MAX(dim_date[Date]), -12), -12, MONTH))
RETURN DIVIDE(Last12M - Prior12M, Prior12M)
```

---

## Delivery Performance

```dax
SLA Breach Rate % =
DIVIDE(
    CALCULATE([Total Deliveries], fact_deliveries[Status] = "Gecikmiş"),
    [Total Deliveries]
)

Avg Delivery Time (hrs) = AVERAGE(fact_deliveries[ActualHours])
```

---

## Driver & Fleet Analysis

```dax
Avg Driver Rating = AVERAGE(fact_deliveries[CustomerRating])

Deliveries per Driver = DIVIDE([Total Deliveries], DISTINCTCOUNT(fact_deliveries[DriverID]))
```

---

## Customer & SLA Insights

```dax
Failed Deliveries = CALCULATE([Total Deliveries], fact_deliveries[Status] = "Uğursuz")

Failure Rate % = DIVIDE([Failed Deliveries], [Total Deliveries])
```

---

## Profitability & Simulation

Requires a **What-If Parameter** first:
`Modeling → New Parameter → Numeric range` — Name: `Fuel Price Multiplier`, Min: 0.7, Max: 1.5,
Increment: 0.05, Default: 1.0

```dax
Simulated Fuel Cost =
SUMX(fact_deliveries, fact_deliveries[FuelCost]) * SELECTEDVALUE('Fuel Price Multiplier'[Fuel Price Multiplier], 1)

Simulated Total Cost = [Simulated Fuel Cost] + SUM(fact_deliveries[DeliveryCost])

Simulated Profit = [Total Revenue] - [Simulated Total Cost]

Simulated Margin % = DIVIDE([Simulated Profit], [Total Revenue])

Cost per Km = DIVIDE([Total Cost], SUM(fact_deliveries[DistanceKm]))

Revenue per Km = DIVIDE([Total Revenue], SUM(fact_deliveries[DistanceKm]))
```
