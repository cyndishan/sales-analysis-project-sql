# 📊 Sales Performance Analysis (SQL & PowerBI Project)

## 🧠 Project Overview

This project analyzes a fictional retail company's sales data using SQL to uncover trends and performance patterns across stores, time, and products. The goal is to generate insights that can support decision-making for marketing and sales strategies.

## 📂️ Dataset

- Name: Retail Sales DatasetSource: Kaggle - Sales Dataset

- Table names: Brands, Categories, Customers, Order_items, Orders, Products, Staffs, Stocks, Stores

## 🔧 Tools Used

- SQL: For querying and analysis

- DBMS: MySQL 

- PowerBI for visualization

## 📌 Methodology

- Created a new MySQL database to store and organize data from multiple CSV files.

- Defined relational tables based on the dataset structure, ensuring primary and foreign key relationships were established.

- Imported data into MySQL using the Table Data Import Wizard in MySQL Workbench for each table (e.g., Brands, Categories, Stores, etc.).

- Wrote and executed SQL queries to explore key areas such as:

  Total and monthly sales performance

  Best-selling products and categories

  Customer purchase behavior

- Used PowerBI to create interactive visualizations.
  
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
sql
```
SELECT 
    s.store_name,
    ROUND(SUM(oi.list_price * oi.quantity * (1 - oi.discount)), 2) AS total_sales
FROM stores s
JOIN orders o ON s.store_id = o.store_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY s.store_name
ORDER BY total_sales DESC;
```
     
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

## 📈 Dashboard

- Visualizations were created using PowerBI to show:

- Sales trends over time

- Sales by category and location

- Sales of product name

*→ Screenshot available in *![image](https://github.com/user-attachments/assets/6a27c93c-6ac3-4814-b114-a102f86d2005)

## 📌 Key Insights

- 🏬 Baldwin Bikes outperformed the other two stores with a total sales figure of $5.2 million between 2016 and 2018.

- 🚲 The best-selling product was Trek Slash 8 27.5, generating $560K in 2016 alone.

- 📉 Sales trends for all the stores followed a similar pattern:

 1. A rise in sales from 2016 to 2017

 2. A consistent decline from 2017 to the end of 2018, falling below 2016 levels

- 🥇 The top-performing product categories were:

 1. Mountain Bikes – $2.7 million

 2. Road Bikes – $1.7 million

 3. Cruisers Bicycles – $1.0 million

## ✅ Actionable Suggestions

🔍 Investigate the sales decline in 2018, especially for Baldwin and Santa Cruz stores. Analyze internal and external factors like pricing, customer satisfaction, or market competition.

🎯 Focus future marketing and inventory on top-performing categories like Mountain Bikes and Road Bikes to maximize return.

🛒 Restock and promote best-sellers such as the Trek Slash 8 27.5, or explore product updates to reignite sales.

📆 Consider launching seasonal or Q4 campaigns, as early trends suggest stronger year-end performance.

## 📬 Contact

**Cyndi Li Shan**
*Aspiring Junior Data Analyst cyndi.shan0101@gmail.com | linkedin.com/in/li-shan-80980a1b9 | github.com/cyndishan*
