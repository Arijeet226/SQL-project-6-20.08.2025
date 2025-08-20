# SQL-project-6-20.08.2025
This is a project made from SQL using advance queries SQL advance Project Challenge Online Food Delivery App This week organized by DataPencil and Kalyani Bhatnagar ma'am special thanks to all who are associated with it for helping me out throughout this journey and letting me showcase my analytical mindset and sharpen my SQL skill which will form as building blocks of my career journey.
DAY-1-SQL JOINs (across 2 to 4 tables)GROUP BY and HAVING clauses COUNT, SUM, AVG and DISTINCT Sorting and filtering logic
DAY-2- Conditional logic using CASE WHEN Table joins across customer, orders, restaurants Aggregation using COUNT, SUM, GROUP BY, ORDER BY Sorting and filtering logic for clean reports.
DAY-3-Writing inner subqueries inside WHERE, SELECT, and FROM clauses
DAY-4- ROW_NUMBER(), RANK(), DENSE_RANK(),SUM() OVER, AVG() OVER, COUNT() OVER Creating running totals & customer activity trends Partitioning & ordering within window functions.Tracking first/last order, most loyal customers, and daily order value trends
DAY-5-More in depth focus on Windows functions.
DAY-6-We're diving deep into SQL Views – a powerful tool every analyst must master!
DAY-7-We’re diving into SQL Temporary Tables – a smart way to store intermediate results and make multi-step queries easier to manage.
You’ll learn how to create temp tables on the fly, use them for complex transformations, and clean them up once you’re done – just like in real-world analytics projects.
DAY-8- SQL CTEs (Common Table Expressions) Special Define a CTE with WITH cte_name AS (...)Chain multiple CTEs to build logic step‑by‑step
Reuse a CTE within the same query (without rewriting subqueries)
Write beginner-friendly non‑recursive CTEs and get a peek at recursive CTEs for hierarchies & sequences
DAY-9-SQL Joins + CTEs + Aggregation + Ranking Special. Blend data from multiple tables with INNER JOIN, LEFT JOIN, and RIGHT JOIN
Use CTEs to simplify and layer your business logic Perform advanced aggregation for KPIs like revenue, orders, and customer activity Apply ranking functions like ROW_NUMBER(), RANK(), and DENSE_RANK() to create leaderboards & trends Mix all concepts into single, production-ready SQL reports
DAY-10-the power of automation in SQL using Stored Procedures Create stored procedures for repeatable business logic Pass parameters to filter results dynamically Handle multiple steps inside a single procedure.
Reuse procedures across different reports without rewriting code.
Automate frequent queries like revenue reports, customer searches, and date-based filters.
DAY-11,12-Practicing the previously taught concept.
DAY-13-14- Focuses on creating the visualization of each queries result and a final report carved out of Canva to summarize all the day's work in a single document. It was a fun journey

# Summary queries


## Q1-Reward tier on customer based on number of order placed
```sql
SELECT c.customer_id,c.customer_name,COUNT(o.order_id) AS total_orders,
CASE 
WHEN COUNT(o.order_id) >= 10 THEN 'GOLD'
WHEN COUNT(o.order_id) BETWEEN 5 AND 9 THEN 'SILVER'
ELSE 'BRONZE'
END AS customer_tier
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/_COUNT%20OF%20CUSTOMER_TIER.png)


## Q2-Restaurant size category
```sql
SELECT resturant_id,COUNT(item_id) AS total_menu_items,
CASE 
WHEN COUNT(item_id) < 5 THEN 'Small'
WHEN COUNT(item_id) BETWEEN 5 AND 10 THEN 'Medium'
ELSE 'Large'
END AS resturant_size_type
FROM menu_item
GROUP BY resturant_id;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/Count%20of%20Resturant_size_type.png)


## Q3-How many high value orders
```sql
SELECT order_category,COUNT(*) AS total_order_count
FROM (SELECT o.order_id,
SUM(m.price * od.quantity) AS order_total,
CASE
WHEN SUM(m.price * od.quantity) > 500 THEN 'High Value'
ELSE 'Low Value'END AS order_category
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
JOIN menu_item m ON od.item_id = m.item_id
GROUP BY o.order_id) AS order_totals
GROUP BY order_category;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/Total_Order_Count.png)


## Q4-For each city, list the restaurant with highest total revenue
```sql
SELECT r.rest_name,r.city
FROM resturant r
WHERE (r.resturant_id,r.city)IN(SELECT r.resturant_id,r2.city
FROM resturant r2
JOIN orders o ON r2.resturant_id=o.resturant_id
JOIN order_details od ON o.order_id=od.order_id
JOIN menu_item m ON od.item_id=m.item_id
GROUP BY r2.city,r2.resturant_id
HAVING SUM(m.price*od.quantity)=(SELECT MAX(total_revenue)
FROM (SELECT r3.city AS city_name,r3.resturant_id,SUM(m.price*od.quantity) AS total_revenue
FROM resturant r3
JOIN orders o ON r3.resturant_id=o.resturant_id
JOIN order_details od ON o.order_id=od.order_id
JOIN menu_item m ON od.item_id=m.item_id
GROUP BY r3.city,r3.resturant_id) AS city_revenue
WHERE city_revenue.city_name=r2.city));
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/image.png)


## Q5-Rank restaurant by total revenue without gap
```sql
SELECT  resturant_id,rest_name,total_revenue,
DENSE_RANK() OVER (ORDER BY total_revenue DESC) AS revenue_rank
FROM (SELECT r.resturant_id,r.rest_name,SUM(m.price * od.quantity) AS total_revenue
FROM resturant r
JOIN menu_item m ON r.resturant_id = m.resturant_id
JOIN order_details od ON m.item_id = od.item_id
GROUP BY r.resturant_id, r.rest_name) AS revenue_per_restaurant
ORDER BY revenue_rank, resturant_id;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/Total%20Revenue%20by%20Restaurant.png)


## Q6-Monthly order summary
```sql
WITH monthly_orders AS 
(SELECT MONTH(order_date) AS order_month,COUNT(order_id) AS total_orders
FROM orders GROUP BY MONTH(order_date))

SELECT order_month,total_orders
FROM monthly_orders
WHERE total_orders > 50
ORDER BY order_month;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/Total%20Order%20Month%20wise.png)


## Q7-Popular items
```sql
CREATE TEMPORARY TABLE temp_popular_item AS 
SELECT m.item_id,m.item_name,COUNT(od.quantity) AS quantity_sold
FROM menu_item m
JOIN order_details od ON m.item_id=od.item_id
GROUP BY m.item_id,m.item_name;

-- show the top 5 items by quantity
SELECT * FROM temp_popular_item
ORDER BY quantity_sold DESC
LIMIT 5;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/TOP%205%20item%20sold%20%20by%20Quantity%20.png)


## Q8-Top N customer by orders
```sql
DELIMITER //
CREATE PROCEDURE topcustomer (IN limit_num INT)
BEGIN 
SELECT c.customer_name,COUNT(o.order_id) AS total_order
FROM customers c
JOIN orders o ON c.customer_id=o.customer_id
GROUP BY c.customer_name
ORDER BY total_order DESC
LIMIT limit_num;
END //
DELIMITER ;
CALL topcustomer (5);
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/TOP%205%20Customers%20by%20Orders.png)


## Q9-Customer who never ordered
```sql
WITH active_customers AS 
(SELECT DISTINCT o.customer_id
FROM orders o)

SELECT c.customer_id,c.customer_name
FROM customers c
LEFT JOIN active_customers ac ON c.customer_id = ac.customer_id
WHERE ac.customer_id IS NULL;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/customer_id.png)


## Q10-Number of item count per restaurant
```sql
WITH item_per_rest AS 
(SELECT m.resturant_id,COUNT(*) AS item_count
FROM menu_item m
GROUP BY m.resturant_id)

SELECT r.rest_name,i.item_count
FROM item_per_rest i
JOIN resturant r ON r.resturant_id=i.resturant_id
ORDER BY i.item_count DESC;
```
![](https://github.com/Arijeet226/SQL-project-6-20.08.2025/blob/15e2cc044b2f4c37d4ca62cd3f1d85d6f849d500/visualization/Item%20Count%20by%20Restaurant.png)
