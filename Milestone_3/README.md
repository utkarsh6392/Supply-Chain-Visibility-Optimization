# 🚚 Transportation & Supplier Analytics Dashboard

An end-to-end analytics project designed to evaluate supplier performance, analyze transportation cost efficiencies, benchmark carrier reliability, and deliver actionable supply chain optimizations.

---

## 📄 Overview

This repository contains data models, DAX measures, and visual analytical frameworks for tracking transportation logistics and supplier performance. The goal is to identify cost-saving opportunities, reduce late shipments, and optimize carrier selection across global markets.

---

## 📊 Performance & Calculation Methodologies

### 1. Supplier Scorecard Calculation Methodology

The **Supplier Overall Score** is calculated using a multi-criteria weighted scoring model across key operational pillars:

$$\text{Supplier Score} = (W_1 \times \text{OTD}) + (W_2 \times \text{Defect Rate Score}) + (W_3 \times \text{Cost Competitiveness})$$

#### Core Metrics & Weights:

* **On-Time Delivery Rate (OTD) [Weight: 40%]**
  $$\text{OTD \%} = \left( \frac{\text{Total On-Time Deliveries}}{\text{Total Completed Orders}} \right) \times 100$$
* **Defect / Return Rate [Weight: 35%]**
  $$\text{Defect Rate \%} = \left( \frac{\text{Total Damaged / Returned Units}}{\text{Total Delivered Units}} \right) \times 100$$
* **Average Discount & Cost Efficiency [Weight: 25%]**
  Evaluates supplier pricing compliance against baseline market targets and contract margins.

---

### 2. Supplier Ranking and Benchmarking Approach

Suppliers and shipping modes are benchmarked across a four-tier performance system based on their overall calculated score:

| Performance Tier | Score Range | Classification | Action / Recommendation |
| :--- | :--- | :--- | :--- |
| **Tier 1** | $90\% - 100\%$ | **Preferred** | Eligible for long-term contract lock-ins and higher volume allocation. |
| **Tier 2** | $75\% - 89\%$ | **Standard** | Meets baseline operational requirements; target for minor optimizations. |
| **Tier 3** | $60\% - 74\%$ | **Underperforming** | Requires active performance improvement plan (PIP) and closer audit. |
| **Tier 4** | $< 60\%$ | **High-Risk** | Phase out or renegotiate SLAs immediately. |

#### Benchmarking Metrics:
* **Peer Group Analysis:** Suppliers are compared against regional averages (`Market` & `Sub-region`) to eliminate geographic bias.
* **Volume-Weighted Profitability:** Ranks carriers not just on raw speed, but on net profit contribution per shipment.

---

### 3. Transportation Cost Analysis Methodology

Transportation cost efficiency is measured by balancing unit cost, order volume, and discount structures across shipping classes (**Standard Class**, **Second Class**, **First Class**, **Same Day**).

#### Key Measures:

* **Average Cost / Profit Per Order:**
  $$\text{Avg Profit Per Order} = \frac{\sum \text{Total Profit}}{\text{Total Orders}}$$

* **Effective Discount Rate (%):**
  $$\text{Avg Discount Rate \%} = \frac{\sum \text{Discount Amount}}{\sum \text{Gross Order Value}}$$

* **Cost-to-Volume Ratio:**
  Evaluates whether volume-heavy shipping channels (e.g., *Standard Class*) achieve expected economies of scale without eroding total profit margins.

---

### 4. Route and Carrier Performance Evaluation

Routes and shipping modes are analyzed continuously using a two-factor matrix: **Reliability** vs. **Cost Impact**.
