# Adventureworks-powerbi
Interactive Power BI report analyzing AdventureWorks sales with KPIs (Total Sales, Profit, Margin %, AOV) and time-series insights (YoY, YTD, MoM). Explore performance by product category, customer segment, and region using slicers, drillthrough, and tooltips.
## Project Summary
This repository contains a production-style **Power BI** report built on **AdventureWorks** data. It helps stakeholders track sales performance and drill from an executive snapshot into detailed analysis across **products**, **customers**, and **regions**. The model follows a star-schema, and the DAX layer implements transparent time-intelligence for **YoY**, **YTD**, and **MoM** views.

## Datasets Used
- **AdventureWorks** sample dataset   

## Data Model (Star Schema)
- **Fact**: Internet Sales (transactions; amounts, quantities, cost).  
- **Dimensions**: `Date` (marked as Date table), `Product` (Category/Subcategory), `Customer`, `Geography`.  
- **Practices**: single-direction relationships where possible; only necessary columns loaded; correct data types and summarizations; calculated columns kept minimal; measures favored over columns.

## Key Dashboards & Features

### 1) Executive Overview
- KPIs: **Total Sales**, **Total Profit**, **Margin %**, **Average Order Value (AOV)**  
- **Time series** with YoY/YTD and seasonality  
- Slicers for **Channel**, **Category**, **Region**, **Date**  
- Bookmarks to toggle common executive views (e.g., YTD vs. YoY)

### 2) Products
- Drill path: **Category → Subcategory → Product**  
- **Top-N** ranking by Sales/Profit with dynamic threshold  
- Contribution analysis to identify **margin drivers** and long-tail products

### 3) Customers
- Segmentation by **education**, **marital status**, **region** (if available in your model)  
- Cohort-style views (e.g., **first purchase month**) and **AOV distribution**  
- Indicators for **repeat purchase** behavior and high-value segments

### 4) Geography
- Interactive **map** with state/country rollups  
- Regional **under/over-performance** vs. prior period  
- Drillthrough to region detail (city/store) with context-preserving filters

## DAX Measures (examples)
> Keep measures small, composable, and well-named. Mark your Date table.

- **Sales**  
  `Sales = SUM(FactInternetSales[SalesAmount])`
- **Profit**  
  `Profit = [Sales] - SUM(FactInternetSales[TotalProductCost])`
- **Margin %**  
  `Margin % = DIVIDE([Profit], [Sales])`
- **AOV (Average Order Value)**  
  `AOV = DIVIDE([Sales], DISTINCTCOUNT(FactInternetSales[SalesOrderNumber]))`
- **Sales YTD**  
  `Sales YTD = TOTALYTD([Sales], 'Date'[Date])`
- **Sales YoY**  
  `Sales YoY = CALCULATE([Sales], DATEADD('Date'[Date], -1, YEAR))`
- **Sales MoM %**  
  `Sales MoM % = DIVIDE([Sales] - CALCULATE([Sales], DATEADD('Date'[Date], -1, MONTH)), CALCULATE([Sales], DATEADD('Date'[Date], -1, MONTH)))`

## Insights & Business Value
- Quantifies **momentum** via YoY/YTD/MoM trends and seasonality.  
- Reveals **assortment** and **pricing** impacts by product hierarchy.  
- Highlights **high-value customers** and **retention** opportunities.  
- Surfaces **regional** pockets of growth/decline for targeted action.

