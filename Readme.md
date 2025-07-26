# Sales Insights Data Analysis Project

## Project Overview

This project involves analyzing sales data from a company operating primarily within India, with some isolated transactions in international locations like New York and Paris. The goal is to clean the data, perform analysis, and derive insights to help the company understand its sales patterns, customer interactions, and product performance.

The analysis primarily focuses on querying the database to clean, filter, and extract useful insights such as total sales, transactions by market, and product performance.

### Database Schema

1. **Customers Table**  
   Contains information about customers:
   - `customer_code`: Unique identifier for the customer.
   - `customer_name`: Name of the customer.
   - `customer_type`: Type of customer (e.g., Regular, VIP, etc.).

2. **Date Table**  
   Contains information related to transaction dates:
   - `date`: Actual date of the transaction.
   - `cy_date`: Date in fiscal year format.
   - `year`: Year of the transaction.
   - `month_name`: Name of the month.
   - `date_yy_mmm`: Date formatted as `yy-mmm` (e.g., `20-Jan`).

3. **Markets Table**  
   Contains information about the markets:
   - `market_code`: Unique identifier for the market.
   - `market_name`: Name of the market (e.g., Chennai, Mumbai).
   - `zone`: Geographical zone of the market.

4. **Products Table**  
   Contains information about products:
   - `product_code`: Unique identifier for the product.
   - `product_type`: Type/category of the product.

5. **Transactions Table**  
   Contains transaction details:
   - `product_code`: Code of the product sold.
   - `customer_code`: Code of the customer who made the purchase.
   - `market_code`: Code of the market where the transaction occurred.
   - `order_date`: Date of the transaction.
   - `sales_qty`: Quantity of products sold.
   - `sales_amount`: Revenue from the transaction.
   - `currency`: Currency used in the transaction (e.g., INR, USD).

## Data Cleaning

The data contains some inconsistencies that need to be addressed:
1. **Garbage Data**: Some transactions are from locations outside India, such as New York and Paris. These records are irrelevant to the analysis and should be filtered out.
2. **Negative Sales Amounts**: Some records in the transactions table have a negative sales amount. These need to be addressed, either by removing or correcting the records.
3. **Currency Conversion**: Transactions in USD need to be converted to INR for consistent analysis.

## SQL Queries for Primary Analysis

### 1. Total Number of Transactions
```sql
SELECT count(*) 
FROM sales.transactions;
```
Result: 150,283 records

2. Total Number of Customers
```sql
SELECT count(*) 
FROM sales.customers;
```
Result: 38 records

3. Transactions from Chennai
```sql
SELECT * 
FROM sales.transactions
WHERE market_code = 'Mark001';
```
Result: 1,035 records

4. Transactions in USD
```sql

SELECT * 
FROM sales.transactions 
WHERE currency = 'USD';
```
Result: 2 records

5. Transactions in 2020
```sql

SELECT sales.transactions.*, sales.date.* 
FROM sales.transactions 
INNER JOIN sales.date 
ON sales.transactions.order_date = sales.date.date 
WHERE sales.date.year = '2020';
```
All rows provided with transactions done in 2020

6. Total Sales Revenue in 2020
```sql
SELECT SUM(sales.transactions.sales_amount) 
FROM sales.transactions 
INNER JOIN sales.date 
ON sales.transactions.order_date = sales.date.date 
WHERE sales.date.year = '2020';
```

7. Total Sales Revenue in 2020 for Chennai
```sql

SELECT SUM(sales.transactions.sales_amount) 
FROM sales.transactions 
INNER JOIN sales.date 
ON sales.transactions.order_date = sales.date.date 
WHERE sales.date.year = '2020' 
AND sales.transactions.market_code = 'Mark001';
```

8. Distinct Products in Chennai
```sql
SELECT DISTINCT product_code 
FROM sales.transactions 
WHERE market_code = 'Mark001';
```

Setup Instructions
=====================
To run this analysis, you'll need:

1. A MySQL or compatible database running the schema as described.

2. A SQL client (e.g., MySQL Workbench, DBeaver) to run the queries.

3. Data cleansing scripts (optional) to filter out garbage data, convert currencies, and fix negative amounts.

