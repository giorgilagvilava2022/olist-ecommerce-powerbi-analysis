# Olist E-Commerce Analytics | Power BI Dashboard

An interactive, three-page Power BI report analyzing sales, products, sellers, customers and delivery performance of the Brazilian marketplace Olist (September 2016 – August 2018). The report is built on a star-schema data model and answers key business questions about revenue, seller concentration, customer behavior and the impact of delivery on customer satisfaction.

![Dashboard overview](images/1.dashboard_overview.png)

## Headline Numbers

| Total Revenue | Total Orders | Avg Order Value | Unique Customers | Avg Review Score | On-Time Delivery |
|---|---|---|---|---|---|
| R$ 13.5M | 98.2K | R$ 137.4 | 95.0K | 4.09 | 93.2% |

Values are in Brazilian reais (BRL) and shown with the R$ symbol in the visuals.

## Business Questions

- How do revenue and order volume evolve over time?
- Which product categories and price bands drive revenue?
- Which regions and states generate the most sales, and where is customer satisfaction lowest?
- How concentrated is revenue among sellers?
- How does delivery performance affect customer reviews?
- How do customers pay, and when do they place orders?

## Key Insights

- **Revenue is heavily concentrated in the Southeast.** The region generates about R$8.8M (~65%) of total revenue and ~67K of 98K orders. South (R$1.9M) and Northeast (R$1.5M) follow, then Central-West (R$0.9M); North is the smallest at R$0.3M.
- **Customer satisfaction is lowest in the Northeast (3.92)**, followed by the North (3.98), compared with 4.14–4.16 in the Southeast and South.
- **Revenue grew strongly through 2017**, rising from roughly R$0.1M in January to a peak of about R$1.0M in November (about R$0.75M in December). In 2018 it stayed at roughly R$0.85–1.0M per month through August. The drop to zero after that point reflects the end of the dataset's coverage (August 2018), not a real decline.
- **Credit cards dominate payments** (78.9% of revenue), followed by boleto (17.6%) and vouchers (~2.1%). Nearly half of all orders use a single installment.
- **Top categories:** health_beauty (R$1.3M), watches_gifts (R$1.2M), bed_bath_table (R$1.0M) and sports_leisure (R$1.0M).
- **The 100–200 price band earns the most revenue** (about R$3.8M); the cheapest items (under 50) earn the least (about R$1.2M). Items sold in volume are mostly priced below ~R$200, while items above R$400 sell only a handful of units.
- **The seller base is fragmented.** Among 3.1K active sellers, the top seller (Seller 4869f7) accounts for only ~1.7% of revenue, and the top five hold 1.4–1.7% each (~7.7% combined). Four of the top five are located in the state of SP; the second-ranked seller is in BA.
- **Seller size does not guarantee quality.** Among the top five sellers, average review scores range from 3.49 to 4.34 and on-time delivery from 89.5% to 96.6%.
- **Delivery quality strongly drives satisfaction.** Orders delivered on time average roughly 4.3 stars, versus roughly 2.3 for late orders and below 2 for undelivered ones. Overall, 93.2% of orders arrive on time, with an average delivery time of 12.4 days.
- **Repeat purchases are rare:** only 3.0% of customers ordered more than once, and 77.1% of reviews are positive.
- **Order timing:** volume ramps up from about 08:00 and stays high on weekdays between roughly 10:00 and 16:00 (the busiest hour overall is 16:00), with a second peak around 20:00–21:00 early in the week. Activity is lowest overnight, between about 03:00 and 06:00. Monday is the busiest day (16.3% of orders) and Saturday the quietest (11.0%); weekends are noticeably quieter during the day (23% of orders in total), although Sunday evening is busy and peaks around 20:00.

> Figures read from charts (such as price-band revenue, average review by delivery status and freight share) are approximate. Exact values are shown in the tables and in the weekday × hour heatmap.

## Dashboard Pages

### 1. Overview

Revenue, orders, average order value, customers, review score and on-time delivery at a glance, with revenue trend by month and year, revenue by payment type, top 10 categories, a revenue map by state and a regional summary table (revenue, orders and average review score).

### 2. Products & Sellers

Active sellers, average item price and items per order, plus a category treemap, a price-versus-volume scatter plot, a top-5 sellers table (revenue, orders, average review score and on-time delivery %), revenue by price band and freight share by weight class.

![Products and Sellers](images/2.dashboard_sales.png)

### 3. Customers & Delivery

Unique customers, repeat customer rate, on-time delivery, delivery days, average review score and positive review share, with an orders heatmap by weekday and hour, average review by delivery status, orders by maximum installments, and a state-level bubble chart of orders, review score and revenue.

![Customers and Delivery](images/3.dashboard_customers.png)

## Interactivity

- Page navigation buttons (Overview, Products & Sellers, Customers & Delivery)
- Slicers for Region, Year and Category
- Reset filters button
- Cross-filtering between visuals and a Selected Period card (Sep 2016 – Aug 2018 when no filters are applied)

## Data Model

The model follows a star schema with Fact_Sales at the center, supported by dimension tables and two order-level tables for payments and reviews.

![Data model](docs/data_model.png)

| Table | Type | Description |
|---|---|---|
| Fact_Sales | Fact | Order items with price, freight, delivery dates, delivery status and calculated columns (delivery_days, Is Late, Line Total) |
| Dim_Customer | Dimension | Customer ID, city, state and ZIP prefix |
| Dim_Product | Dimension | Product attributes and category |
| Dim_Seller | Dimension | Seller ID, city, state and coordinates |
| Dim_Date | Dimension | Calendar table (month, month number, quarter, weekday) |
| Dim_State | Dimension | State-to-region mapping |
| Payments_Order | Order-level | Payment methods, main payment type, installments and total payment per order |
| Reviews_Order | Order-level | Review score, comment flag and sentiment per order |
| _Measures | Measures | Dedicated table for DAX measures |

Relationships are one-to-many from each dimension (and order-level table) to Fact_Sales; Dim_State filters Dim_Customer.

## KPIs and DAX Measures

The report uses a dedicated `_Measures` table. Main measures include:

| Area | Measures |
|---|---|
| Sales | Total Revenue, Total Orders, Avg Order Value, Avg Item Price, Items per Order |
| Customers | Unique Customers, Repeat Customer %, Positive Review %, Avg Review Score |
| Delivery | On-Time Delivery %, Avg Delivery Days |
| Sellers | Active Sellers, Revenue Share %, Seller Rank |

## Tools & Techniques

- **Power BI Desktop:** report design, dark theme, custom navigation and slicer panel
- **Power Query:** data cleaning, type conversion and building dimension and order-level tables
- **DAX:** measures and calculated columns
- **Data modeling:** star schema with a calendar table and a region mapping
- **Visuals:** line and column charts, donut, treemap, scatter and bubble charts, heatmap matrix, map, tables and KPI cards

## Repository Structure

```
olist-ecommerce-powerbi-analysis/
├── README.md
├── .gitignore
├── data/
│   └── README.md                 # where to download the raw data
├── powerbi/
│   └── Olist_Dashboard.pbix      # Power BI report
├── docs/
│   ├── data_model.png            # data model diagram
│   └── presentation.pdf          # project presentation
└── images/
    ├── dashboard_overview.png
    ├── dashboard_sales.png
    └── dashboard_customers.png
```

## How to Open the Report

1. Install Power BI Desktop (free, Windows).
2. Download `powerbi/Olist_Dashboard.pbix` from this repository.
3. Open the file in Power BI Desktop. The data is embedded in the report, so it opens without extra setup.
4. To refresh the data, download the CSV files from Kaggle (see below), then go to **Transform data → Data source settings** and point the source paths to your local copy.

## Data Source

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) on Kaggle (real, anonymized commercial data, 2016–2018). The dataset is licensed under CC BY-NC-SA 4.0 and is used here for educational and portfolio purposes only. Raw files are not included in this repository because of their size; see `data/README.md`.

## Notes and Limitations

- The report covers September 2016 – August 2018. 2016 contains only a few months of data, and the data ends in August 2018, so the drop to zero afterwards is missing data, not a real decline. This affects year-over-year comparisons.
- The dataset contains no cost or margin data, so profitability is not analyzed.
- Review scores reflect only customers who left a review.

## Author

**Giorgi Lagvilava**
Junior Data Analyst | Power BI
[LinkedIn](LINKEDIN_URL) · [Email](mailto:YOUR_EMAIL)
