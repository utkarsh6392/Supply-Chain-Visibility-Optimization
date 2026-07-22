# Supply Chain Analytics: Inventory & Delivery Performance Dashboard

An end-to-end Power BI analytics solution designed to evaluate inventory health, monitor working capital risk, and optimize fulfillment efficiency. This project combines Inventory Analytics and Delivery Performance to give supply chain managers complete visibility into stock turnover, dead stock prevention, delivery delays, and regional logistics.

---

## Executive Summary

- **Inventory Optimization:** Identified slow-moving and dead stock risks to minimize carrying costs and improve stock turnover.
- **Fulfillment Efficiency:** Compared scheduled and actual shipping times to monitor delivery performance and reduce delays.
- **Interactive Drill-Downs:** Implemented hierarchical analysis for geography (Region → Country → City) and products (Category → Product Name).

---

## Data Model & Schema Overview

The dashboard follows a **Star Schema** with a centralized **Fact Table** containing sales, inventory, and shipping transactions.

### Dimension Hierarchies
- **Geography:** `Region → Order Country → Order City`
- **Product:** `Category Name → Product Name`
- **Time:** `Order Date → Quarter → Year`

---

# Inventory Analytics

## Inventory Turnover Ratio

Measures how efficiently inventory is sold and replenished during a given period.

**Formula**

\[
\text{Inventory Turnover Ratio} =
\frac{\text{Total Sales}}
{\text{Average Inventory Value}}
\]

### DAX

```DAX
Total Sales =
SUM(Fact_table[sales])

Avg Inventory Value =
AVERAGE(Fact_table[inventory_value])

Inventory Turnover Ratio =
DIVIDE([Total Sales], [Avg Inventory Value], 0)
```

**Purpose**
- Evaluate inventory efficiency.
- Identify slow-moving inventory.
- Support inventory planning and optimization.

---

## Days Since Last Sale

Calculates the number of days since each product was last sold by comparing its latest sale date with the latest transaction date in the dataset.

### DAX

```DAX
Days Since Last Sale (Col) =
VAR LastSale =
    CALCULATE(
        MAX(Fact_table[order_date_(dateorders)]),
        ALLEXCEPT(Fact_table, Fact_table[product_name])
    )

VAR MaxDate =
    CALCULATE(
        MAX(Fact_table[order_date_(dateorders)]),
        ALL(Fact_table)
    )

RETURN
    DATEDIFF(LastSale, MaxDate, DAY)
```

**Purpose**
- Detect inactive products.
- Identify dead or slow-moving stock.
- Improve inventory planning.

---

# Delivery Performance Analytics

## On-Time Delivery Rate

Calculates the percentage of orders delivered on time.

### DAX

```DAX
On-Time Delivery Rate =
DIVIDE(
    CALCULATE(
        COUNTROWS(Fact_table),
        Fact_table[Late_delivery_risk] = 0
    ),
    COUNTROWS(Fact_table),
    0
)
```

**Purpose**
- Measure delivery reliability.
- Monitor fulfillment performance.
- Track logistics efficiency.

---

## Average Shipping Days (Actual)

Calculates the average actual shipping duration.

### DAX

```DAX
Avg Shipping Days Real =
AVERAGE(Fact_table[Days for shipping (real)])
```

---

## Average Shipping Days (Scheduled)

Calculates the average planned shipping duration.

### DAX

```DAX
Avg Shipping Days Scheduled =
AVERAGE(Fact_table[Days for shipment (scheduled)])
```

---

## Average Delivery Delay

Measures the average difference between actual and scheduled shipping time.

### DAX

```DAX
Avg Delivery Delay Days =
[Avg Shipping Days Real] -
[Avg Shipping Days Scheduled]
```

**Purpose**
- Quantify shipping delays.
- Compare planned vs. actual delivery performance.
- Highlight logistics bottlenecks.

---

# Dashboard Features

- Inventory Turnover Analysis
- Dead Stock Identification
- Days Since Last Sale Tracking
- On-Time Delivery KPI
- Average Delivery Delay Monitoring
- Shipping Performance Analysis
- Regional Performance Analysis
- Product Category Analysis
- Interactive Drill-through and Filters
- Dynamic KPIs and Visualizations

---

# Business Impact

- Reduced inventory carrying costs through better stock visibility.
- Identified slow-moving and inactive products for inventory optimization.
- Improved delivery performance monitoring using real-time KPIs.
- Enabled data-driven supply chain decisions with interactive dashboards.
- Enhanced operational efficiency through detailed regional and product-level analysis.
