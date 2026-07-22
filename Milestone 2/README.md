# Supply Chain Analytics: Inventory & Delivery Performance Dashboard

An end-to-end Power BI analytics solution designed to evaluate inventory health, monitor working capital risk, and optimize fulfillment efficiency. This project combines Inventory Analytics and Delivery Performance to give supply chain managers complete visibility into stock turnover, dead stock prevention, delivery delays, and regional logistics.

---

## Executive Summary

* **Inventory Optimization:** Identified slow-moving and dead stock risks to minimize carrying costs and increase stock velocity.
* **Fulfillment Efficiency:** Benchmarked scheduled vs. actual shipping days to reduce late delivery risk across regions and shipping modes.
* **Interactive Drill-Downs:** Implemented multi-level location (Region -> Order Country -> Order City) and product (Category -> Product Name) hierarchies for granular analysis.

---

## Data Model & Schema Overview

The solution utilizes a star schema data structure centered around a consolidated fact table containing sales, shipping, and inventory transactions.

### Key Dimensions & Hierarchies
* **Geography Hierarchy:** `region` -> `order_country` -> `order_city`
* **Product Hierarchy:** `category_name` -> `product_name`
* **Temporal Axis:** `order_date_(dateorders)` mapped across fiscal quarters and years

---

## Inventory Analytics Methodology

### 1. Inventory Turnover Calculation Approach
The Inventory Turnover Ratio measures how efficiently inventory is sold and replaced over a given period. To avoid distortion from point-in-time stock spikes, the ratio uses average inventory valuation:

$$ \text{Inventory Turnover Ratio} = \frac{\text{Total Sales}}{\text{Average Inventory Value}} $$

* **DAX Implementation:**
  ```dax
  Total Sales = SUM(Fact_table[sales])

  Avg Inventory Value = AVERAGE(Fact_table[inventory_value])

  Inventory Turnover Ratio = 
  DIVIDE([Total Sales], [Avg Inventory Value], 0)

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


    On-Time Delivery Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_table), Fact_table[Late_delivery_risk] = 0),
    COUNTROWS(Fact_table),
    0
)


Avg Shipping Days Real = AVERAGE(Fact_table[Days for shipping (real)])

Avg Shipping Days Scheduled = AVERAGE(Fact_table[Days for shipment (scheduled)])

Avg Delivery Delay Days = [Avg Shipping Days Real] - [Avg Shipping Days Scheduled]



On-Time Delivery Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_table), Fact_table[Late_delivery_risk] = 0),
    COUNTROWS(Fact_table),
    0
)
