# Assignment1_Tumwine-Keneth_20252SEN344
Sunrise Supermarket
# PL/SQL Assignment One - Sunrise Supermarket

# MY INFORMATION

Name: Tumwine Keneth  
Student ID: 20252SEN344
Course: Database Development with PL/SQL  
DBMS:PostgreSQL

# 1. Business Scenario

Sunrise Supermarket sells products to customers who place
orders containing one or more items.

The purpose of this database assignment is to help management
understand customers, products, purchases and sales trends over time.

The database contains:

- 5 customers
- 8 products
- 4 product categories
- 15 orders
- 25 order items

# 2. Database Used

This assignment was developed using PostgreSQL.

The database contains four main tables:
- customers
- products
- orders
- order_items

# 3. JOIN Queries

# Query 1: Orders and Customers

This query uses INNER JOIN to display each order together
with the customer's name, city and order date.

-- Q1
SELECT
o.order_id,c.customer_name,c.city,o.order_date
FROM orders o INNER JOIN customers c ON o.customer_id = c.customer_id ORDER BY o.order_id;

# Business Interpretation
It helps management identify who placed each order,
where the customer is located and when the order was placed.

# Query 2: Order Items and Products
This query uses INNER JOIN to display product name,
category, price and quantity.

-- Q2
SELECT oi.order_item_id,p.product_name,p.category,p.price,oi.quantity
FROM order_items oi INNER JOIN products p
ON oi.product_id = p.product_id ORDER BY oi.order_item_id;


# Business Interpretation
It helps management understand which products customers
are purchasing and the quantities purchased.

# Query 3: Customers and Orders
This query uses LEFT JOIN to display all customers,
including customers who have no orders.

-- Q3
SELECT c.customer_id,c.customer_name,c.city,o.order_id,o.order_date
FROM customers c LEFT JOIN orders o ON c.customer_id = o.customer_id ORDER BY c.customer_id, o.order_date;

# Business Interpretation
It helps management identify active customers and customers
who have not yet placed orders.

# 4. CTE Query
The CTE calculates the total amount spent by each customer.

The query then compares each customer's total spending
with the average spending.

-- Q1
WITH customer_totals AS(
SELECT c.customer_id,c.customer_name, SUM(oi.quantity * p.price) AS total_spend
FROM customers c JOIN orders o ON c.customer_id = o.customer_id JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id GROUP BY c.customer_id,c.customer_name)
SELECT customer_id,customer_name,total_spend
FROM customer_totals WHERE total_spend>(SELECT AVG(total_spend) FROM customer_totals) ORDER BY total_spend DESC;


# Business Interpretation
This helps management identify customers whose spending
is above the average.

# 5. Window Functions
# 5.1 Customer Spending Rank
RANK() is used to rank customers according to their total spending.

-- Q1

WITH customer_totals AS(SELECT c.customer_id,c.customer_name,
SUM(oi.quantity*p.price) AS total_spend FROM customers c JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id JOIN products p ON oi.product_id = p.product_id
GROUP BY c.customer_id,c.customer_name)
SELECT customer_id,customer_name,total_spend, RANK() OVER(ORDER BY total_spend DESC) AS spending_rank
FROM customer_totals ORDER BY spending_rank;

# Business Interpretation
Management can identify customers based on their spending levels.

# 5.2 Customer Order Number
ROW_NUMBER() is used to number each customer's orders
according to the order date.

-- Q2
SELECT customer_id,order_id,order_date,
ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS order_number
FROM orders ORDER BY customer_id, order_date;

# Business Interpretation
This shows the sequence of purchases made by each customer.

# 5.3 Running Revenue
SUM() OVER() is used to calculate the running total of revenue
over time.

-- Q3
WITH order_revenue AS (SELECT o.order_id,o.order_date,
SUM(oi.quantity * p.price) AS order_total FROM orders o JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id GROUP BY o.order_id,o.order_date)
SELECT order_id,order_date,order_total, SUM(order_total) OVER(ORDER BY order_date, order_id) AS running_revenue
FROM order_revenue ORDER BY order_date, order_id;

# Business Interpretation
Management can monitor how sales revenue grows over time.

# 5.4 Days Between Orders
LAG() is used to obtain the previous order date for each customer.

-- Q4
WITH order_dates AS (SELECT customer_id,order_id,order_date, LAG(order_date) OVER (PARTITION BY customer_id
ORDER BY order_date) AS previous_order_date FROM orders)
SELECT customer_id,order_id,order_date,previous_order_date,order_date - previous_order_date AS days_between_orders
FROM order_dates WHERE previous_order_date IS NOT NULL ORDER BY customer_id, order_date;

# Business Interpretation
This helps management understand how frequently customers return.


## 7. Challenges and Resolutions

### Challenge 1
Understanding how different tables are connected.

# Resolution
I used primary keys and foreign keys to establish relationships
between customers, orders, products and order_items.

# Challenge 2
Understanding window functions.

# Resolution
I practiced RANK(), ROW_NUMBER(), SUM() OVER() and LAG()
with the order data.

# Challenge 3
Calculating customer spending.

# Resolution
I used quantity multiplied by product price and grouped
the results by customer.
