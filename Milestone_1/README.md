# Supply Chain Visibility & Optimization — Milestone 1: Data Modelling

## Objective
* Build a clean, reliable data foundation for supply chain analytics.
* Import the DataCo Smart Supply Chain dataset into Power BI.
* Clean and transform the raw data using Power Query.
* Design a robust star schema data model with fact and dimension tables optimized for KPI reporting.

---

## Dataset Source
* **Dataset:** DataCo Smart Supply Chain for Big Data Analysis (Kaggle)
* **Link:** [Kaggle Dataset URL](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)

---

## Data Cleaning and Transformation Steps
* **Data Ingestion:** Loaded the raw CSV into Power BI via `Get Data > Text/CSV` and opened the Power Query Editor.
* **Query Setup:** Renamed the primary source query to `Fact_table`.
* **Data Privacy & Cleanup:** Removed sensitive and redundant columns, including `Customer Email`, `Customer Password`, and `Order Zipcode`.
* **Quality Assurance:** Leveraged *Column Quality* and *Column Distribution* tools to identify missing values and check for duplicate records.
* **Data Type Correction:** Standardized column formats strictly according to data types:
  * Dates updated to **Date/Time**
  * Prices/Financials updated to **Decimal Number**
  * IDs/Quantities updated to **Whole Number**
* **Dimension Table Creation:** Built seven specific dimension tables (`Dim_Customer`, `Dim_Product`, `Dim_Category`, `Dim_Department`, `Dim_Shipping`, and `Dim_Location`) by duplicating the main query, retaining only relevant columns, and removing duplicate rows to ensure uniqueness.
* **Surrogate Keys:** Created unique surrogate keys (`shipping_id`, `Location_id`) using **Index Columns**, mapping them back into the `Fact_table` via merge operations.
* **Calendar Table:** Generated a dedicated `Dim_Date` table using the DAX `CALENDAR()` function, creating calculated attributes for **Year**, **Month**, **Quarter**, **Week**, **Day**, and **Day Name**.

---

## Data Model Overview
* **Architecture:** Implemented a standardized **Star Schema** with `Fact_table` at the absolute center, housing core transactional metrics (sales, profit, quantity, dates, and surrogate keys).
* **Relationships:** Connected the central fact table to seven distinct dimension tables:
  * `Dim_Customer`
  * `Dim_Product`
  * `Dim_Category`
  * `Dim_Department`
  * `Dim_Shipping`
  * `Dim_Location`
  * `Dim_Date` *(Configured with one active relationship on order date and one inactive relationship on shipping date).*
* **Schema Diagram:** For a visual map of the relationships, please refer to `screenshots/Data_model.png`.

---

## Tools Used
* **Power BI Desktop:** Data import, Power Query transformations, star schema modeling, and DAX measures.
* **Microsoft Excel / CSV:** Source data parsing and structural review.
* **GitHub:** Version control, project tracking, and documentation management.

---

## Author
* **Utkarsh Pratap Singh**
