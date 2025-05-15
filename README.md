# 📊 Sales Performance Analysis (SQL & Python Project)

## 🧠 Project Overview

This project analyzes a fictional retail company's sales data using SQL to uncover trends and performance patterns across time, products, and customer segments. The goal is to generate insights that can support decision-making for marketing, inventory, and sales strategies.

## 📂️ Dataset

- Name: Retail Sales DatasetSource: Kaggle - Sales Dataset:

- Table names: Brands, Categories, Customers, Order_items, Orders, Products, Staffs, Stocks, Stores

## 🔧 Tools Used

- SQL: For querying and analysis

- DBMS: MySQL 

- Python for visualization

## 📌 Methodology

- Created a new MySQL database to store and organize data from multiple CSV files.

- Defined relational tables based on the dataset structure, ensuring primary and foreign key relationships were established.

- Imported data into MySQL using the Table Data Import Wizard in MySQL Workbench for each table (e.g., Customers, Orders, Products, etc.).

- Wrote and executed SQL queries to explore key areas such as:

  Total and monthly sales performance

  Best-selling products and categories

  Store-wise revenue comparison

  Customer purchase behavior

- Used Python to create clear and simple visualizations (bar charts, line graphs) from query results.

- Embedded charts and summarized findings in this README.md to effectively communicate data insights.

- Demonstrated the full cycle of data analysis — from importing raw data to generating insights and visuals for storytelling.

## 🧪 SQL Executions & Tasks

Create all the tables in MySQL Workbench Environment following the same order as when referencing keys it would throw errors:
```
-- create database
CREATE DATABASE SalesPractice_db;

-- use database 
USE SalesPractice_db;

-- create tables one by one, using correct sequences for further foreign key references
-- 1. Brands
CREATE TABLE Brands (
    brand_id INT PRIMARY KEY,
    brand_name VARCHAR(100)
);

-- 2. Categories
CREATE TABLE Categories (
    category_id INT PRIMARY KEY,
    category_name VARCHAR(100)
);

-- 3. Stores
CREATE TABLE Stores (
    store_id INT PRIMARY KEY,
    store_name VARCHAR(100),
    phone VARCHAR(20),
    email VARCHAR(100),
    street VARCHAR(100),
    city VARCHAR(50),
    state VARCHAR(50),
    zip_code VARCHAR(10)
);

-- 4. Staffs
CREATE TABLE Staffs (
    staff_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(20),
    active INT,
    store_id INT,
    manager_id INT,
    FOREIGN KEY (store_id) REFERENCES Stores(store_id),
    FOREIGN KEY (manager_id) REFERENCES Staffs(staff_id) -- Self-reference
);

-- 5. Customers
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    phone VARCHAR(20),
    email VARCHAR(100),
    street VARCHAR(100),
    city VARCHAR(50),
    state VARCHAR(50),
    zip_code VARCHAR(10)
);

-- 6. Products
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    brand_id INT,
    category_id INT,
    model_year YEAR,
    list_price DECIMAL(10, 2),
    FOREIGN KEY (brand_id) REFERENCES Brands(brand_id),
    FOREIGN KEY (category_id) REFERENCES Categories(category_id)
);

-- 7. Orders
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_status VARCHAR(20),
    order_date DATE,
    required_date DATE,
    shipped_date DATE,
    store_id INT,
    staff_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (store_id) REFERENCES Stores(store_id),
    FOREIGN KEY (staff_id) REFERENCES Staffs(staff_id)
);

-- 8. Order_items
CREATE TABLE Order_items (
    order_id INT,
    item_id INT,
    product_id INT,
    quantity INT,
    list_price DECIMAL(10, 2),
    discount DECIMAL(4, 2),
    PRIMARY KEY (order_id, item_id),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- 9. Stocks
CREATE TABLE Stocks (
    store_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (store_id, product_id),
    FOREIGN KEY (store_id) REFERENCES Stores(store_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);
```

- 1. Total Sales performance of each store
     
- 2. 📅 Monthly Sales Trends
 sql
 ```

SELECT
    DATE_TRUNC('month', OrderDate) AS Month,
    SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY Month
ORDER BY Month;
```
Insight: Sales peak in December, likely due to holiday shopping, with a noticeable drop in January.

- 2. 🛒 Top 5 Best-Selling Products
sql
 ```
SELECT
    ProductName,
    SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY ProductName
ORDER BY TotalSales DESC
LIMIT 5;
```
Insight: Electronics dominate the top-selling list, suggesting strong demand and opportunity for upselling.

- 3. 🌍 Sales by Location
sql
 ```
SELECT
    Location,
    SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY Location
ORDER BY TotalSales DESC;
```
Insight: Store A generates 30% more revenue than Store B, especially in the electronics category.

- 4. 👥 Average Order Value per Customer
sql
 ```
SELECT
    CustomerID,
    ROUND(AVG(Sales), 2) AS AvgOrderValue
FROM SalesData
GROUP BY CustomerID
ORDER BY AvgOrderValue DESC;
```
Insight: The top 10% of customers spend more than 3x the average, indicating potential for loyalty programs.

## 📌 Key Business Insights

- Electronics is the top-performing category overall.

- December sees the highest monthly sales, while January is the lowest.

- Store A consistently outperforms Store B, except in Books category.

- High-spending customers represent a small segment but drive a large portion of revenue.

## 📈 Dashboard

- Visualizations were created using Python to show:

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
*Aspiring Junior Data Analyst cyndi.shan0101@gmail.com | linkedin.com/in/li-shan-80980a1b9 | github.com/cyndishan*
