# E-Commerce-Sales-Performance-PowerBI-Dashboard
A comprehensive sales performance dashboard built in Power BI.

## Project Overview
A comprehensive Sales Performance dashboard built using Power BI. This project aims to provide actionable insights into sales trends, customer behavior, product performance to support data-driven decision-making.

## Key Features & Analysis
- **Sales Performance Overview:** Monthly sales trends, sales by category, total sales.
- **Customer Insights:** Customer acquisition trend, top customers by sales, customer lifetime value.
- **Product Analysis:** Top-selling products, Total units sold by category, product ranking.
- **Time Intelligence Deep Dive:** Quarter-over-Quarter comparisons, Year-to-Date (YTD) trends, Month-over-Month (MoM) growth.

## Technologies Used
- **Power BI Desktop:** For data modeling, DAX calculations, and visualization.
- **DAX:** For creating complex measures (e.g., `SalesLastYear`, `SalesYTD`, `MonthOverMonthGrowth%`, `ProductSalesRank`).
- **Data Source:** https://www.kaggle.com/datasets/chadwambles/ecommerce-transactions

## Data Model
"A Star Schema with a central 'Transactions' fact table and 'Products', 'Customers', and 'DateTable' dimension tables."

## Key Insights
- Insight 1: The Books and Electronics categories collectively account for over 50% of total sales.
- Insight 2: Top 5 products contribute 19% of total revenue, highlighting their importance.
- Insight 3: Customer acquisitions peaked at 80 in January 2024; however, the trend subsequently declined significantly, requiring further investigation.


## Visuals (Screenshots)
<img width="632" alt="E-Commerce Sales Performance Page 1" src="https://github.com/user-attachments/assets/b60be60a-4d6b-4244-bf2b-70d9145605e7" />

*This page provides a high-level overview of total sales and order trends, allowing for quick assessment of overall business health.*


<img width="633" alt="E-Commerce Sales Performance Page 4" src="https://github.com/user-attachments/assets/63a2987b-b1a5-4433-bfca-dc844f779216" />

*This page provides Month-over-Month Sales Growth percentage to track short-term sales momentum and identify periods of significant change.*
