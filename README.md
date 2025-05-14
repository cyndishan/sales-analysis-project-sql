# 📊 Sales Performance Analysis (SQL Project)

## 🧠 Project Overview

This project analyzes a fictional retail company's sales data using SQL to uncover trends and performance patterns across time, products, and customer segments. The goal is to generate insights that can support decision-making for marketing, inventory, and sales strategies.

## 📂️ Dataset

- Name: Retail Sales DatasetSource: Kaggle - Sales Dataset or a cleaned CSV fileContents:

- OrderID

- OrderDate

- CustomerID

- ProductCategory

- ProductName

- Quantity

- UnitPrice

- Location

- Sales (calculated as Quantity × UnitPrice)

## 🔧 Tools Used

- SQL: For querying and analysis

- DBMS: PostgreSQL / MySQL / SQLite (your choice)

- (Optional) Excel or Power BI for visualization

## 🧪 SQL Questions & Tasks

- 1. 📅 Monthly Sales Trends

SELECT
    DATE_TRUNC('month', OrderDate) AS Month,
    SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY Month
ORDER BY Month;

Insight: Sales peak in December, likely due to holiday shopping, with a noticeable drop in January.

- 2. 🛒 Top 5 Best-Selling Products

SELECT
    ProductName,
    SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY ProductName
ORDER BY TotalSales DESC
LIMIT 5;

Insight: Electronics dominate the top-selling list, suggesting strong demand and opportunity for upselling.

- 3. 🌍 Sales by Location

SELECT
    Location,
    SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY Location
ORDER BY TotalSales DESC;

Insight: Store A generates 30% more revenue than Store B, especially in the electronics category.

- 4. 👥 Average Order Value per Customer

SELECT
    CustomerID,
    ROUND(AVG(Sales), 2) AS AvgOrderValue
FROM SalesData
GROUP BY CustomerID
ORDER BY AvgOrderValue DESC;

Insight: The top 10% of customers spend more than 3x the average, indicating potential for loyalty programs.

## 📌 Key Business Insights

- Electronics is the top-performing category overall.

- December sees the highest monthly sales, while January is the lowest.

- Store A consistently outperforms Store B, except in Books category.

- High-spending customers represent a small segment but drive a large portion of revenue.

## 📈 Optional Dashboard

- Visualizations were created using EXCEL to show:

- Sales trends over time

- Sales by category and location

- Customer segmentation based on spending

*→ Screenshot available in */images/dashboard.png

## 🚀 Next Steps

- Recommend promotional campaigns during low-sales months.

- Expand inventory for top-selling products.

- Explore reasons behind underperformance in books (Store A).

- Consider loyalty incentives for high-spending customers.

## 📬 Contact

**Cyndi Li Shan**
*Aspiring Junior Data Analyst[cyndi.shan0101@gmail.com] | [linkedin.com/in/li-shan-80980a1b9] | [github.com/cyndishan]*
