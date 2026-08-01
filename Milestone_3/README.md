# Transportation & Supplier Analytics Dashboard

An end-to-end supply chain analytics project designed to evaluate supplier performance, analyze transportation cost efficiencies, benchmark carrier reliability, and deliver actionable operational optimizations.

---

## Overview

This repository contains data models, DAX measures, and interactive visual frameworks for tracking global transportation logistics and vendor performance. The objective of this project is to provide data-driven insights that identify cost-saving opportunities, reduce order latency, optimize carrier selection, and mitigate supply chain bottlenecks across global markets.

---

## Data Pipeline & Architecture

1. **Data Ingestion & Cleaning:** Raw transactional shipping data and vendor records are imported, sanitized, and normalized in Power Query (removing missing values, standardizing dates, and mapping market codes).
2. **Data Modeling:** Built using a standard **Star Schema** to optimize query performance and DAX aggregation speed.
   - **Fact Table:** `Fact_Shipments` (Order ID, Ship Date, Freight Cost, Discounts, Profit, Delivery Status)
   - **Dimension Tables:** `Dim_Supplier`, `Dim_ShippingMode`, `Dim_Geography`, `Dim_Calendar`
3. **Analytics Engine:** Custom DAX measures for dynamic time-intelligence, weighted benchmarking, and tiered KPI metrics.

---

## Performance & Calculation Methodologies

### 1. Supplier Scorecard Calculation Methodology

The **Supplier Overall Score** is calculated using a multi-criteria weighted scoring model across three core operational pillars:

$$\text{Supplier Score} = (W_1 \times \text{OTD}) + (W_2 \times \text{Defect Rate Score}) + (W_3 \times \text{Cost Competitiveness})$$

#### Core Metrics & Weights:

* **On-Time Delivery Rate (OTD) [Weight: 40%]**
  $$\text{OTD \%} = \left( \frac{\text{Total On-Time Deliveries}}{\text{Total Completed Orders}} \right) \times 100$$

* **Defect / Return Rate [Weight: 35%]**
  $$\text{Defect Rate \%} = \left( \frac{\text{Total Damaged / Returned Units}}{\text{Total Delivered Units}} \right) \times 100$$

* **Average Discount & Cost Efficiency [Weight: 25%]**
  Evaluates vendor contract adherence and pricing competitiveness relative to baseline market standard rates.

---

### 2. Supplier Ranking and Benchmarking Approach

Suppliers and shipping modes are benchmarked across a four-tier performance framework based on their cumulative weighted score:

| Performance Tier | Score Range | Classification | Action / Recommendation |
| :--- | :--- | :--- | :--- |
| **Tier 1** | $90\% - 100\%$ | **Preferred** | High priority for volume allocation and multi-year contract renewals. |
| **Tier 2** | $75\% - 89\%$ | **Standard** | Meets baseline operational requirements; target for minor SLA tweaks. |
| **Tier 3** | $60\% - 74\%$ | **Underperforming** | Subject to Performance Improvement Plans (PIPs) and active audit tracking. |
| **Tier 4** | $< 60\%$ | **High-Risk** | Transition allocation away or terminate active vendor agreements. |

#### Benchmarking Metrics:
* **Peer Group Normalization:** Vendors are benchmarked within their specific geographic segment (`Market` & `Sub-region`) to eliminate geographic latency bias.
* **Volume-Weighted Profitability:** Ranks logistics providers based on net profit contribution per shipment rather than speed alone.

---

### 3. Transportation Cost Analysis Methodology

Transportation cost efficiency is evaluated by assessing the interaction between unit costs, volume distributions, and applied discount structures across shipping classes (**Standard Class**, **Second Class**, **First Class**, **Same Day**).

#### Key Measures & Formulas:

* **Average Profit Per Order:**
  $$\text{Avg Profit Per Order} = \frac{\sum \text{Total Profit}}{\text{Total Orders}}$$

* **Effective Discount Rate (%):**
  $$\text{Avg Discount Rate \%} = \frac{\sum \text{Discount Amount}}{\sum \text{Gross Order Value}}$$

* **Cost-to-Volume Ratio:**
  Evaluates whether high-volume shipping modes (e.g., Standard Class) achieve target economies of scale without eroding total profit margins.

---

### 4. Route and Carrier Performance Evaluation

Routes and shipping modes are continuously monitored on a two-factor axis: **Reliability (Late Rate)** vs. **Profit Contribution**.
