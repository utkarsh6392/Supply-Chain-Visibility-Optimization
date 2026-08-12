# Supply Chain Visibility & Optimization Dashboard

## Overview
This document provides full documentation and technical architecture for the Supply Chain Visibility and Optimization Power BI solution[cite: 1]. The project features executive-level KPIs, operational tracking, warehouse efficiency analytics, automated narrative summaries, and interactive drill-through capabilities[cite: 1].

---

## Technical Visual Architecture & Dax Measures

### 1. Key Performance Indicator (KPI) Cards
The upper KPI deck tracks high-level metrics across fulfillment, inventory, and supplier activity[cite: 1].

* **Total Orders**
  * Expression: `Total Orders = DISTINCTCOUNT(Fact_table[order_id])`[cite: 1]
  * Value Displayed: `7.104K`[cite: 1]

* **Count of Product Categories**
  * Expression: `Category Count = DISTINCTCOUNT(Fact_table[product_category_id])`[cite: 1]
  * Value Displayed: `42`[cite: 1]

* **Fulfillment Rate**
  * Expression: `Fulfillment Rate = DIVIDE([Total Orders] - [Canceled Orders], [Total Orders], 0)`[cite: 1]
  * Formatting: Percentage (`95.97%`)[cite: 1]

* **Total Warehouses**
  * Expression: `Total Warehouses = DISTINCTCOUNT(Dim_warehouse[warehouse_id])`[cite: 1]
  * Value Displayed: `10`[cite: 1]

* **Total Suppliers**
  * Expression: `Total Suppliers = DISTINCTCOUNT(Dim_supplier[supplier_id])`[cite: 1]
  * Value Displayed: `118`[cite: 1]

* **Dead Stock Quantity**
  * Expression:
    ```dax
    Dead Stock Quantity = 
    SUMX(
        FILTER(
            VALUES(Fact_table[product_name]),
            CALCULATE(MAX(Fact_table[Stock Status])) = "Dead Stock"
        ),
        CALCULATE(AVERAGE(Fact_table[stock_qty]))
    )
    ```[cite: 1]
  * Value Displayed: `5.51255M`[cite: 1]

---

### 2. Gauge Charts (KPI Metrics vs Targets)

* **Late Delivery %**
  * DAX Calculations:
    ```dax
    Late Orders = CALCULATE(DISTINCTCOUNT(Fact_table[order_id]), Fact_table[delivery_status] = "Late delivery")
    Late Delivery % = DIVIDE([Late Orders], [Total Orders], 0)
    Late Delivery % Target = 0.20
    ```[cite: 1]
  * Axis Setup: Min = `0.00`, Max = `1.00`, Target Value = `0.20`[cite: 1]

* **Average of Reliability %**
  * Source Field: `AVERAGE(Dim_supplier[reliability_%])`[cite: 1]
  * Axis Setup: Min = `0`, Max = `100`[cite: 1]

* **Average of Utilization %**
  * DAX Calculation:
    ```dax
    Warehouse Utilization Target = 80
    ```[cite: 1]
  * Source Field: `AVERAGE(Dim_warehouse[utilization_%])`[cite: 1]
  * Axis Setup: Min = `0`, Max = `100`, Target Value = `80`[cite: 1]

* **Inventory Turnover Ratio**
  * DAX Calculations:
    ```dax
    Total Inventory Value = 
    SUMX(
        VALUES(Fact_table[product_name]), 
        CALCULATE(AVERAGE(Fact_table[inventory_value]))
    )

    Inventory Turnover Ratio = DIVIDE([Total Sales], [Total Inventory Value], 0)
    ```[cite: 1]
  * Custom Formatting: `0.00x`[cite: 1]

---

## Executive Dashboard Design Methodology

The executive layout is structured following visual hierarchy best practices to support rapid decision-making[cite: 1]:

1. **Top Row (Primary Metrics):** High-level KPI cards providing immediate status on overall throughput, capacity, supplier base, and dead stock risk[cite: 1].
2. **Second Row (Performance Gauges):** Operational health monitors tracking SLA compliance, supplier reliability, warehouse utilization, and capital movement against pre-defined targets[cite: 1].
3. **Bottom Left (Trend & Time Series Analysis):** Line chart illustrating historical revenue trajectory alongside statistical forecasts[cite: 1].
4. **Bottom Center (Automated Smart Narrative & Global Slicers):** AI-generated text highlighting statistical drivers (percentage increases/decreases across date ranges) paired with interactive filters for market, product category, and date parameters[cite: 1].
5. **Bottom Right (Regional Breakdown Table):** Summary matrix displaying regional revenue distribution, serving as the launch point for cross-report drill-through actions[cite: 1].

---

## Drill-Through Implementation & Configuration Steps

The drill-through feature enables users to seamlessly pivot from summary metrics on the Executive dashboard (`Page 1`) to detailed order itemization on the `order_details` page[cite: 1].

### Configuration Procedure:

1. **Create the Target Page:**
   * Add a new page and rename it to `order_details`[cite: 1].

2. **Configure Drill-Through Filters:**
   * Select the page background on `order_details`[cite: 1].
   * In the Visualizations pane, locate the **Drill-through** bucket[cite: 1].
   * Drag `Fact_table[order_region]` into the field well[cite: 1].
   * Set **Keep all filters** to **On**[cite: 1].
   * Set **Cross-report** to **On**[cite: 1].
   * Power BI automatically adds a Back Button (`<-`) in the upper-left corner of the canvas[cite: 1].

3. **Build the Detail Visual:**
   * Place a Table visual on the `order_details` page[cite: 1].
   * Add fields: `order_id`, `order_date_(dateorders)`, `product_name`, `order_region`, `delivery_status`, `customer_segment`, and `Sum of sales`[cite: 1].

4. **Execution & Navigation:**
   * Navigate back to the Executive dashboard[cite: 1].
   * Right-click any region entry in the `order_region` summary table[cite: 1].
   * Select **Drill through** -> **order_details**[cite: 1].
   * Use **Ctrl + Click** on the Back Button on the `order_details` page to return to the Executive view[cite: 1].

---

## Warehouse Utilization Calculation Methodology

Warehouse utilization is measured at both granular warehouse levels and aggregated network levels to pinpoint capacity bottlenecks[cite: 1]:

* **Granular Utilization:**
  $$\text{Warehouse Utilization \%} = \left( \frac{\text{Current Stock Volume}}{\text{Total Storage Capacity}} \right) \times 100$$

* **Network Min, Max, and Average Metrics:**
  * **Max Utilization %:** `MAX(Dim_warehouse[utilization_%])` (Identifies saturated facilities operating near capacity risk thresholds)[cite: 1].
  * **Min Utilization %:** `MIN(Dim_warehouse[utilization_%])` (Identifies underutilized facilities with excess holding capacity)[cite: 1].
  * **Avg Utilization %:** `AVERAGE(Dim_warehouse[utilization_%])` (Measures network-wide volumetric load)[cite: 1].

* **Capacity Risk Flag Logic:**
  Facilities operating above 80% utilization trigger a capacity risk flag to initiate inventory rebalancing across adjacent facilities[cite: 1].

---

## Throughput and Productivity KPI Calculations

Throughput metrics evaluate order processing speed and volumetric output across storage facilities[cite: 1]:

* **Warehouse Units Shipped:**
  * Calculation: `SUM(Fact_table[units_shipped])`[cite: 1]
  * Measures total volumetric output per facility[cite: 1].

* **Units per Order (Warehouse Productivity):**
  * Calculation:
    ```dax
    Units per Order = DIVIDE(SUM(Fact_table[units_shipped]), [Total Orders], 0)
    ```[cite: 1]
  * Represents pick-density and packing efficiency[cite: 1].

* **Order Processing Density:**
  * Measures sales output generated per warehouse unit processed[cite: 1]:
    ```dax
    Sales per Unit Shipped = DIVIDE([Total Sales], SUM(Fact_table[units_shipped]), 0)
    ```[cite: 1]

---

## Forecasting Implementation Approach

The revenue trajectory line chart uses Power BI built-in exponential smoothing algorithms to forecast future demand patterns[cite: 1]:

1. **Date Granularity Standardizing:**
   * Create calculated month-start attribute in Power Query / DAX[cite: 1]:
     ```dax
     Order Month = DATE(YEAR(Fact_table[order_date]), MONTH(Fact_table[order_date]), 1)
     ```[cite: 1]
2. **Visual Setup:**
   * X-axis: `Order Month`[cite: 1]
   * Y-axis: `Total Sales`[cite: 1]
3. **Analytics Pane Configuration:**
   * Enable **Trend Line** to display baseline direction[cite: 1].
   * Enable **Forecast**[cite: 1]:
     * Forecast Length: `6 Months`[cite: 1]
     * Seasonality: Automatic (or set to `12` for annual cycle matching)[cite: 1]
     * Confidence Interval: `95%` (renders confidence shade around future projections)[cite: 1]

---

## Dashboard Optimization Techniques

To ensure fast query response times and smooth visual rendering, the model incorporates several performance optimization practices[cite: 1]:

1. **Star Schema Data Modeling:** Star schema structure separating fact records (`Fact_table`) from dimensional lookups (`Dim_warehouse`, `Dim_supplier`)[cite: 1].
2. **Measure-Based Aggregations:** Avoiding calculated columns for aggregations; using explicit DAX measures (`SUMX`, `DIVIDE`, `CALCULATE`) to minimize stored memory footprint[cite: 1].
3. **Optimized DAX Division:** Enforcing `DIVIDE(numerator, denominator, alternate_result)` across all ratio measures to avoid divide-by-zero errors and memory-intensive `IF` checks[cite: 1].
4. **Slicer & Visual Interaction Optimization:** Disabling unnecessary cross-highlighting across non-related visual containers to decrease overall DAX engine query cycles[cite: 1].

---

## Key Insights and Business Recommendations

### 1. Delivery & SLA Compliance
* **Observation:** Late delivery rate currently sits at 51.2%, significantly missing the 20.0% operational target[cite: 1].
* **Action:** Investigate carrier bottlenecks in high-volume regions (`Central America` and `Southeast Asia`) and adjust lead-time estimates on order commitments[cite: 1].

### 2. Capital Lockup in Dead Stock
* **Observation:** Over 5.51M units are classified under dead stock, binding active capital and reducing inventory turnover (0.47x)[cite: 1].
* **Action:** Launch promotional clearance initiatives for dead stock categories to release working capital and optimize warehouse holding capacity[cite: 1].

### 3. Warehouse Utilization Imbalance
* **Observation:** Peak facility utilization reaches 84.24% while minimum facility utilization drops to 15.93% (average 55.15%)[cite: 1].
* **Action:** Re-route inbound logistics allocations from saturated facilities (e.g., L4, L3) to underutilized warehouses (e.g., L0, L2, L6) to balance operational pressure[cite: 1].
