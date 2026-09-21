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

# Business Interpretation
It helps management identify who placed each order,
where the customer is located and when the order was placed.

# Query 2: Order Items and Products
This query uses INNER JOIN to display product name,
category, price and quantity.

# Business Interpretation
It helps management understand which products customers
are purchasing and the quantities purchased.

# Query 3: Customers and Orders
This query uses LEFT JOIN to display all customers,
including customers who have no orders.

# Business Interpretation
It helps management identify active customers and customers
who have not yet placed orders.

# 4. CTE Query
The CTE calculates the total amount spent by each customer.

The query then compares each customer's total spending
with the average spending.

# Business Interpretation
This helps management identify customers whose spending
is above the average.

# 5. Window Functions
# 5.1 Customer Spending Rank
RANK() is used to rank customers according to their total spending.

# Business Interpretation
Management can identify customers based on their spending levels.

# 5.2 Customer Order Number
ROW_NUMBER() is used to number each customer's orders
according to the order date.

# Business Interpretation
This shows the sequence of purchases made by each customer.

# 5.3 Running Revenue
SUM() OVER() is used to calculate the running total of revenue
over time.

# Business Interpretation
Management can monitor how sales revenue grows over time.

# 5.4 Days Between Orders
LAG() is used to obtain the previous order date for each customer.

# Business Interpretation
This helps management understand how frequently customers return.

# 6. Results
Screenshots of the query results are included in this repository.

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
