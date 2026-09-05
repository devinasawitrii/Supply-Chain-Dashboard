# Supply Chain Performance Dashboard

A performance dashboard analyzing daily, SKU-level supply chain operations across inventory and sales dimensions for fiscal year 2024.

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Dashboard](#dashboard)
- [Key Insights](#key-insights)
- [Data Quality Notes](#data-quality-notes)
- [Tools & Stack](#tools--stack)
- [Repository Structure](#repository-structure)
- [How to Reproduce](#how-to-reproduce)
---

## Overview

This project explores and visualizes a simulated supply chain dataset spanning one full year of daily activity. The analysis covers inventory health (stock levels, reorder points, replenishment behavior, stockout risk) and sales performance (demand vs. forecast, profitability, top-performing SKUs) across multiple warehouses, suppliers, and regions.

The end deliverable is a two-page interactive dashboard supported by exploratory data analysis (EDA) documented in an accompanying notebook.

## Dataset

**Source:** [High-Dimensional Supply Chain Inventory Dataset](https://www.kaggle.com) (Kaggle).

The dataset simulates daily SKU-level supply chain operations, including sales, dynamic inventory levels, supplier lead times, replenishment orders, regional distribution, promotions, and stockout indicators.

| Attribute | Value |
|---|---|
| Time range | Jan 1, 2024 – Dec 30, 2024 |
| Total records | 91,250 |
| Unique SKUs | 50 |
| Warehouses | 5 |
| Suppliers | 10 |
| Regions | 4 (North, East, South, West) |

### Schema

| Column | Type | Description |
|---|---|---|
| `Date` | datetime | Transaction date |
| `SKU_ID` | categorical | Product identifier |
| `Warehouse_ID` | categorical | Warehouse identifier |
| `Supplier_ID` | categorical | Supplier identifier |
| `Region` | categorical | Distribution region |
| `Units_Sold` | numeric | Units sold on that date |
| `Inventory_Level` | numeric | On-hand stock level |
| `Supplier_Lead_Time_Days` | numeric | Delivery lead time from supplier |
| `Reorder_Point` | numeric | Inventory threshold that triggers replenishment |
| `Order_Quantity` | numeric | Units ordered when replenishment occurs |
| `Unit_Cost` | numeric | Cost per unit |
| `Unit_Price` | numeric | Selling price per unit |
| `Promotion_Flag` | binary | Whether a promotion was active |
| `Stockout_Flag` | binary | Whether a stockout occurred |
| `Demand_Forecast` | numeric | Forecasted demand for planning |

## Dashboard

The dashboard is organized into two pages.

### 1. Inventory

| Metric | Value |
|---|---|
| Total Inventory | 43,026,411 units |
| Stockout Events | 0 |

- **Stock Levels Relative to Reordering Thresholds** — time series comparing on-hand inventory against the reorder point band.
- **Inventory Distribution by Warehouse** — donut chart of inventory share across WH_1–WH_5.
- **Sales and Inventory Performance Across Warehouses** — combo chart comparing sales value and inventory level per warehouse.
- **Inventory Position and Replenishment Needs by SKU** — scatter plot of inventory level vs. reorder point per SKU, used to flag replenishment priority.
- **Top 3 SKUs Furthest Above Reorder Point** and **Top 3 SKUs Closest to Reorder Point** — ranked bar charts supporting replenishment prioritization.

### 2. Sales

| Metric | Value |
|---|---|
| Total Units Sold | 1,829,979 |
| Total Sales | 33.4M (currency unit) |
| Profit | 11.1M (currency unit) |
| Profit Margin | 33.2% |

- **Actual Demand and Forecast Trends** — time series of `Units_Sold` vs. `Demand_Forecast` over the year.
- **Best Selling SKUs** — top 3 SKUs by units sold (SKU_18, SKU_1, SKU_33).
- **Profit Contribution by SKU** — treemap showing relative profit contribution across all 50 SKUs.

*Filters:* both pages support filtering by `SKU_ID` and by date range (default: full year 2024).

## Key Insights

- **Zero stockouts** were recorded across the entire observation period, suggesting the reorder-point policy is functioning effectively at current demand levels.
- **Inventory is evenly distributed** across the five warehouses, ranging narrowly from 19.3% to 20.8% of total stock.
- **Demand follows a seasonal pattern**, peaking mid-year and tapering toward Q4, closely tracked by the forecast model.
- **Profit margin averages 33.2%**, indicating a reasonably efficient cost-to-price structure across the SKU portfolio.
- A small number of SKUs (e.g., SKU_5, SKU_14, SKU_17) consistently appear at the extremes of the reorder-point scatter plot, making them useful bellwethers for inventory policy review.

## Data Quality Notes

Findings from the exploratory analysis notebook:

- No duplicate rows and no negative values were found across any numeric column.
- `Stockout_Flag` is constant at `0` for all 91,250 records — it carries no variance and is therefore not usable as a predictive target in its current form.
- `Order_Quantity` has a median of `0`, since replenishment only triggers on the specific days a SKU crosses its reorder point.
- All categorical dimensions (`SKU_ID`, `Warehouse_ID`, `Supplier_ID`, `Region`) are fully populated with the expected cardinality (50 / 5 / 10 / 4).

## Tools & Stack

- **Python** — data loading, cleaning, and exploratory analysis
- **Google Data Studio** — visualization and dashboard layer

## Repository Structure

```
.
├── data/supply_chain_dataset1.csv
├── notebook/eda.ipynb
├── dashboard/Supply_Chain_Performance_Dashboard.pdf
└── README.md
```

