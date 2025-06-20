




















# Sales-by-Customers-orders-Analysis
📁 sql-sales-analysis/
├── customers.csv         # Synthetic customers data
├── orders.csv # Synthetic orders data
├── queries.sql           # All SQL queries used
├── analysis_summary.md   # Summary insights from the analysis
└── README.md # Project documentation (this file)

📊 SQL Sales Analysis Project

This project features a practical SQL analysis of customer and sales data using two core datasets: customers.csv and orders.csv. It aims to showcase the use of SQL for business intelligence tasks like trend analysis, customer segmentation, revenue tracking, and performance reporting.

📁 Dataset Descriptions

customers.csv

This file contains synthetic data for 500 customers across different U.S. regions. It represents a simplified customer database.

Column Name
Description
customer_id
Unique identifier for each customer
customer_name
Full name of the customer
email
Email address of the customer
region
State/region the customer is from

orders.csv

This file includes over 3,000 records of customer orders placed over a 2-year period. Each row corresponds to a single order.

Column Name
Description
order_id
Unique ID for each order
customer_id
Foreign key linking to the customer
order_date
Date when the order was placed
total_amount
Total value of the order in USD

🧠 SQL Skills Demonstrated
	•	Data Joining: INNER JOIN, LEFT JOIN
	•	Aggregations: SUM(), AVG(), COUNT()
	•	Window Functions: RANK(), DENSE_RANK(), ROW_NUMBER()
	•	Time Analysis: MONTH(), YEAR(), DATE_FORMAT()
	•	Customer segmentation (new vs repeat)
	•	Subqueries and CTEs

 SQL Queries
 
SELECT 
C.customer_id,
c.name,
SUM(O.amount) AS Total_spent
FROM customers_dataset C
INNER JOIN orders_dataset O 
ON C.customer_id = O.order_id
GROUP BY C.customer_id,C.name
ORDER BY Total_spent DESC ; 


SELECT 
C.customer_id,
c.name,
c.email
FROM customers_dataset C
LEFT JOIN 
orders_dataset O 
ON C.customer_id= O.customer_id
WHERE O.order_id IS NULL ;

SELECT 
customer_id,name
FROM customers_dataset
WHERE customer_id IN (SELECT customer_id
FROM orders_dataset
GROUP BY customer_id
HAVING SUM(amount) >100000);

WITH Rankedorders AS (SELECT *,ROW_NUMBER() OVER (PARTITION BY Customer_id ORDER BY order_date DESC) AS rn
FROM orders_dataset)
SELECT *
FROM Rankedorders
ORDER BY customer_id,rn;


SELECT c.country,
o.status,
COUNT(*) AS order_count
FROM orders_dataset o
JOIN customers_dataset c ON o.customer_id=C.customer_id
GROUP BY C.country,o.status
ORDER BY c.country,order_count DESC;



SELECT TOP 5 C.Customer_id,c.name,Total_spent
FROM(SELECT customer_id,SUM(amount) AS Total_spent
FROM orders_dataset
GROUP BY customer_id) AS sub JOIN customers_dataset C ON sub.customer_id=sub.customer_id
ORDER BY Total_spent DESC;

SELECT 
c.customer_id,
c.name,
COUNT(o.order_id) AS Total_order,
AVG(o.amount) AS Avg_total_value
FROM customers_dataset c 
LEFT JOIN orders_dataset o  ON C.customer_id= o.order_id
GROUP BY C.customer_id,C.name
ORDER BY Total_order DESC;

SELECT *
FROM customers_dataset
WHERE customer_id IN (SELECT customer_id FROM orders_dataset WHERE amount= (SELECT MAX(amount) FROM orders_dataset));

SELECT*
FROM customers_dataset
WHERE customer_id IN (SELECT customer_id FROM orders_dataset WHERE amount = (SELECT MIN(amount) FROM orders_dataset));
 
SELECT order_date,COUNT(*) AS orders_per_day
FROM orders_dataset
GROUP BY order_date
ORDER BY orders_per_day;

SELECT 
country,SUM(amount)AS country_sales
FROM customers_dataset
JOIN orders_dataset ON customers_dataset.customer_id= orders_dataset.customer_id
GROUP BY country
ORDER BY country_sales DESC ;







