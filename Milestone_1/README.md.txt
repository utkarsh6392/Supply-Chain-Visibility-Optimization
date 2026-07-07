# Supply Chain Visibility & Optimization — Milestone 1: Data Modelling
## Objective
Build a clean, reliable data foundation for supply chain analytics by importing
the DataCo Smart Supply
Chain dataset into Power BI, cleaning and transforming it in Power Query, and
designing a star schema
data model with fact and dimension tables ready for KPI reporting.
## Dataset Source
DataCo Smart Supply Chain for Big Data Analysis (Kaggle):
https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-d
ata-analysis
## Data Cleaning and Transformation Steps- Loaded the raw CSV into Power BI via Get Data > Text/CSV, then opened Power
Query Editor.- Renamed the main query to Fact_table.- Removed sensitive/unneeded columns: Customer Email, Customer Password, Order
Zipcode.- Checked column quality and column distribution to identify missing values, and
reviewed the data for
  duplicate rows.- Corrected data types for each column (dates to Date/Time, prices to Decimal
Number, ids to Whole Number).- Built dimension tables (Dim_Customer, Dim_Product, Dim_Category,
Dim_Department, Dim_Shipping,
  Dim_Location) by duplicating Fact_table, keeping only relevant columns, and
removing duplicate rows.- Created surrogate keys (shipping_id, Location_id) using Index columns, then
merged them back into
  Fact_table.- Built a Dim_Date calendar table using DAX CALENDAR(), with Year, Month,
Quarter, Week, Day and Day
  Name columns.
## Data Model Overview
A star schema with Fact_table at the centre (sales, profit, quantity, dates, ids)
connected to seven
dimension tables: Dim_Customer, Dim_Product, Dim_Category, Dim_Department,
Dim_Shipping, Dim_Location,
and Dim_Date (one active relationship on order date, one inactive relationship on
shipping date). See
screenshots/Data_model.png for the full diagram.
## Tools Used- Power BI Desktop (data import, Power Query transformations, data modelling, DAX
measures)- Microsoft Excel / CSV (source data format)- GitHub (version control and project submission)
## Author
Utkarsh Pratap Singh
