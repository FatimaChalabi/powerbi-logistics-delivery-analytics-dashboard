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
