# 🛒 Retail Sales Database Analysis using SQL (January 2025)

This project showcases SQL-based analysis of a Northwind-style retail database. It emphasizes extracting sales, customer, inventory, and supplier insights using advanced SQL queries involving JOINs, window functions, CASE statements, and aggregations.

---

## 📌 Project Overview

- **Client/Scenario:** Simulated retail & wholesale supply chain environment
- **Goal:** Provide business intelligence insights to optimize sales, customer engagement, and supply operations
- **Technology:** SQL (PostgreSQL / MySQL / SQL Server)

---

## 🎯 Problem Statement

The retail company seeks to improve performance in:

- Sales tracking by product and region
- Customer segmentation by spend behavior
- Supplier evaluation by delivery metrics
- Inventory optimization and stock levels
- Sales representative performance monitoring

---

## 🔍 Tasks & Query Examples

### ✅ Task 1: Identify top 5 selling products by revenue
```sql
SELECT p.product_name, 
       SUM(od.unit_price * od.quantity) AS total_revenue
FROM order_details od
JOIN products p ON od.product_id = p.product_id
GROUP BY p.product_name
ORDER BY total_revenue DESC
LIMIT 5; 
```

### ✅ Task 2: Average order value per customer
```sql
SELECT c.customer_id, c.company_name, 
       AVG(od.unit_price * od.quantity) AS avg_order_value
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_details od ON o.order_id = od.order_id
GROUP BY c.customer_id, c.company_name;
```

### ✅ Task 3: Classify customers as Low/Medium/High spenders
```sql
SELECT customer_id,
       SUM(unit_price * quantity) AS total_spent,
       CASE
         WHEN SUM(unit_price * quantity) < 1000 THEN 'Low'
         WHEN SUM(unit_price * quantity) BETWEEN 1000 AND 5000 THEN 'Medium'
         ELSE 'High'
       END AS spend_category
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
GROUP BY customer_id;
```

### ✅  Task 4: Rank sales reps by monthly order volume
```sql
SELECT e.employee_id, e.first_name || ' ' || e.last_name AS employee_name,
       COUNT(o.order_id) AS total_orders,
       RANK() OVER (ORDER BY COUNT(o.order_id) DESC) AS order_rank
FROM employees e
JOIN orders o ON e.employee_id = o.employee_id
WHERE DATE_TRUNC('month', o.order_date) = DATE_TRUNC('month', CURRENT_DATE)
GROUP BY e.employee_id, employee_name;
```

### ✅ Task 5: Stock availability by product category
```sql
SELECT c.category_name, 
       SUM(p.units_in_stock) AS total_units_available
FROM products p
JOIN categories c ON p.category_id = c.category_id
GROUP BY c.category_name;
```

### ✅ Task 6: Inactive customers in the past 6 months
```sql
SELECT c.customer_id, c.company_name
FROM customers c
WHERE c.customer_id NOT IN (
    SELECT DISTINCT o.customer_id
    FROM orders o
    WHERE o.order_date >= CURRENT_DATE - INTERVAL '6 months'
);
```

✅ Task 7: Supplier performance – average delivery time
```sql
SELECT s.supplier_id, s.company_name,
       AVG(DATE_PART('day', required_date - order_date)) AS avg_delivery_days
FROM suppliers s
JOIN products p ON s.supplier_id = p.supplier_id
JOIN order_details od ON p.product_id = od.product_id
JOIN orders o ON od.order_id = o.order_id
GROUP BY s.supplier_id, s.company_name;
```

## 🔧 Technologies Used

- **SQL:** MySQL 
- **Tools:** DBMS tools like pgAdmin, DBeaver, SQL Workbench, Azure Data Studio  
- **Data Model:** Star-schema style Northwind database

---

## 💡 Key Concepts Demonstrated

- Multi-table `JOIN` operations  
- Data categorization using `CASE` statements  
- Ranking & analytics with `WINDOW FUNCTIONS`  
- Subqueries for filtering and comparison  
- Aggregation: `SUM`, `AVG`, `COUNT`, `GROUP BY`

---

## 📈 Outcome

These insights helped simulate real-world retail analysis scenarios including:

- Strategic pricing and inventory restocking  
- Enhanced customer segmentation  
- Performance-based rewards for sales staff  
- Supply chain improvements via delivery tracking
