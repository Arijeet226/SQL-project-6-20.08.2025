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
SELECT r.city,COUNT(o.order_id)AS total_order
FROM orders o
JOIN resturant r
ON o.resturant_id=r.resturant_id
GROUP BY city
ORDER BY total_order DESC;
/*JAIPUR,HYDRABAD,DELHI have the highest number of food orders compared to other city
--active markets
--focus-marketing strategies
--expansion strategies
--improve operations in other cities*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Total_order%20vs.%20city.png)


## Q2-Restaurant size category
```sql
SELECT m.item_name,SUM(m.price*od.quantity) AS total_revenue
FROM menu_item m
JOIN order_details od
ON m.item_id=od.item_id
GROUP BY m.item_name
ORDER BY total_revenue DESC; 
/*insights-best performaing food items
ALOO PARATHA,FISH CURRY,HAKKA NOODLES the top 3 food item generating revenue
promote it more prominently on app
ensure consistent avalaibility and faster delivery*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Total%20revenue%20VS%20Item.png)


## Q3-How many high value orders
```sql
SELECT c.customer_name,SUM(m.price*od.quantity) AS total_revenue
FROM menu_item m
JOIN order_details od
ON m.item_id=od.item_id
JOIN orders o
ON od.order_id=o.order_id
JOIN customers c
ON o.customer_id=c.customer_id
GROUP BY c.customer_name
ORDER BY total_revenue DESC; 
/*MUHAMMAD PATEL, VIHAAN NAIR,VIHAAN PATEl are the customers who spends more than the rest
Get a feedback from them and get to know the reason behind their trust in this platform
list and track their favourite resturant and the food ordered by them and find the pattern and crack it's popularity
Try to implement those readings in the loss making businesses to attract customers.*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Total%20revenue%20VS%20customers.png)


## Q4-For each city, list the restaurant with highest total revenue
```sql
SELECT r.rest_name,COUNT(o.order_id)AS order_count
FROM orders o
JOIN resturant r
ON o.resturant_id=r.resturant_id
GROUP BY rest_name
ORDER BY order_count DESC;
/*GOLDEN GARDEN, SPICE PALACE, TASTY BISTRO are the top restrurant where number of orders are more 
to the underperformers in this list it is suggested to give discount and combo offers to get more order
and study the business approach of the top runner in this list and apply those to improve the order count
To ensure the top runner to remain in this game as toppers try to follow trendy ideas to increase the frequency of order*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/ORDER_COUNT%20%20vs%20RESTURANT.png)


## Q5-Rank restaurant by total revenue without gap
```sql
SELECT r.city,AVG(m.price*od.quantity) AS total_revenue
FROM order_details od
JOIN menu_item m
ON od.item_id=m.item_id
JOIN resturant r
ON m.resturant_id=r.resturant_id
GROUP BY r.city
ORDER BY total_revenue DESC; 
/*MUMBAI,HYDRABAD,PUNE have the highest average order value by city compared to SURAT AHMEDABAD
 AS top runner have active markets ,focus-marketing strategies ,
 expansion strategies , need to improve operations in other cities*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Average%20revenue%20vs.%20city.png)


## Q6-Monthly order summary
```sql
SELECT MONTH(order_date)AS month_number,MONTHNAME(order_date)AS order_month,COUNT(order_id)AS total_orders
FROM orders
GROUP BY MONTH(order_date),MONTHNAME(order_date)
ORDER BY month_number;
/*insights tracking the monthly growth in orders
peak ordering months
impacts of festivals,holidays ,weather
plan time sensitive discount or campaigns*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Total_orders%20vs.%20Order_month.png)


## Q7-Popular items
```sql
SELECT c.city,SUM(m.price*od.quantity) AS total_revenue
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN order_details od
ON o.order_id=od.order_id
JOIN menu_item m
ON od.item_id=m.item_id
GROUP BY c.city
ORDER BY total_revenue DESC
LIMIT 3;
/*CHENNAI,PUNE,BANGALORE are the top 3*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/TOTAL%20REVENUE%20TOP%203%20CITY.png)


## Q8-Top N customer by orders
```sql
SELECT city, COUNT(DISTINCT customer_id) AS number_of_customer
FROM customers
GROUP BY city
ORDER BY number_of_customer DESC;
/*AHMEDBAD, CHENNAI,KOLKATA are the top 3 with most number of customer
try to focus on the least in terms of customers like JAIPUR*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Number_of_customer%20vs.%20city.png)


## Q9-Customer who never ordered
```sql
SELECT m.item_name,SUM(od.order_id)AS total_orders
FROM menu_item m
JOIN order_details od
ON m.item_id=od.item_id
GROUP BY m.item_name
ORDER BY total_orders DESC;
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac994219594a1f0be3a5bd6c5c25/visualizations/Total%20orders%20VS%20Items.png)


## Q10-Number of item count per restaurant
```sql
SELECT r.rest_name,COUNT(o.order_id) AS orders_count
FROM resturant r
JOIN orders o
ON r.resturant_id=o.resturant_id
GROUP BY rest_name
HAVING orders_count < 30
ORDER BY  orders_count ASC;
/* GOLDEN DINER is the lowest among less than 30 order count criteria it has 14 order count
TOTAL 11 resturant are there in this list*/
```
![](https://github.com/Arijeet226/SQL-project-3-20.7.2025/blob/dac1a6d26a9fac9942195)
