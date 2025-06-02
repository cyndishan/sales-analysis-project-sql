# 📊 Sales Performance Analysis (SQL & PowerBI Project)

## 🧠 Project Overview

This project analyzes a bicycle retail company's sales data using SQL to uncover trends and performance patterns across stores, time, and products. The goal is to generate insights that can support decision-making for marketing and sales strategies.

## 📂️ Dataset

- Name: Retail Sales DatasetSource: Kaggle - [Sales Dataset](https://www.kaggle.com/datasets/dillonmyrick/bike-store-sample-database)

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

- 1. 💰Total Sales performance of each store
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
![image](https://github.com/user-attachments/assets/644cdb6f-5f24-407a-b50c-d6d43d76bfd2)

     
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
![image](https://github.com/user-attachments/assets/3623cdae-f08a-4bdc-bc38-0099c98160d4)
![image](https://github.com/user-attachments/assets/43d6c036-12c6-4d72-833e-8a65c4737a64)
![image](https://github.com/user-attachments/assets/43e36ecc-ed45-41cf-98cd-d2f4ece56a8a)
![image](https://github.com/user-attachments/assets/3910c8aa-13e9-43da-b483-c9ffffa338d6)
![image](https://github.com/user-attachments/assets/3e6df770-d0e2-44de-b85c-ba4be1c9a0d8)
![image](https://github.com/user-attachments/assets/d9386400-9bc3-4e58-bd3b-acd624b04fbd)
![image](https://github.com/user-attachments/assets/8fe49e60-2683-4eb7-9a75-3a78b1ba8dff)
![image](https://github.com/user-attachments/assets/508aef2e-2ba5-4d62-a790-7f8c531f675a)


- 3. 🚲 Best selling products
sql
 ```
SELECT 
    p.product_name, 
    SUM(oi.quantity) AS total_units_sold
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_name
ORDER BY total_units_sold DESC
LIMIT 10;
```
![image](https://github.com/user-attachments/assets/43790ce3-6c5d-464a-b033-4b9319658ef4)


- 4. 🔥Best selling categories
sql
 ```
SELECT 
    c.category_name,
    ROUND(SUM(oi.quantity * oi.list_price * (1 - oi.discount)), 2) AS total_sales
FROM 
    Order_items oi
JOIN 
    Products p ON oi.product_id = p.product_id
JOIN 
    Categories c ON p.category_id = c.category_id
GROUP BY 
    c.category_name
ORDER BY 
    total_sales DESC;
```
![image](https://github.com/user-attachments/assets/9f1a4c2e-5cb2-4746-bfbd-ce7af1b49756)


- 5. 🔥Best 3 yearly top selling categories
sql
 ```
SELECT 
    year,
    category_name,
    total_sales
FROM (
    SELECT 
        YEAR(o.order_date) AS year,
        c.category_name,
        ROUND(SUM(oi.quantity * oi.list_price * (1 - oi.discount)), 2) AS total_sales,
        RANK() OVER (
            PARTITION BY YEAR(o.order_date) 
            ORDER BY SUM(oi.quantity * oi.list_price * (1 - oi.discount)) DESC
        ) AS category_rank
    FROM 
        Order_items oi
    JOIN 
        Orders o ON oi.order_id = o.order_id
    JOIN 
        Products p ON oi.product_id = p.product_id
    JOIN 
        Categories c ON p.category_id = c.category_id
    GROUP BY 
        YEAR(o.order_date), c.category_name
) AS ranked_categories
WHERE category_rank <= 3
ORDER BY year, category_rank;
```
![image](https://github.com/user-attachments/assets/b8a4cd11-29e4-4dad-a3d1-16b8fa015d37)


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
