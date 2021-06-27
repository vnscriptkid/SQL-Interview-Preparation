# SQL Practice Problems
## 19 beginning challenges for you to solve using a "learn-by-doing" approach

### 1. Which shippers do we have?
```sql
SELECT * FROM shippers;
```

### 2. Certain fields from Categories
```sql
SELECT
	category_name, 
	description
FROM categories;
```

### 3. Sales Representatives
```sql
SELECT
	first_name,
	last_name,
	hire_date
FROM employees
WHERE title = 'Sales Representative';
```

### 4. Sales Representatives in the United States
```sql
SELECT
	first_name,
	last_name,
	hire_date
FROM employees
WHERE 
	title = 'Sales Representative'
	AND country = 'USA';
```

### 5. Orders placed by specific EmployeeID
```sql
SELECT
	order_id,
	order_date
FROM orders
WHERE employee_id = 5;
```

### 6. Suppliers and ContactTitles
```sql
SELECT
	supplier_id,
	contact_name,
	contact_title
FROM suppliers
WHERE contact_title <> 'Marketing Manager';
```

### 7. Products with “queso” in ProductName
```sql
SELECT
	product_id,
	product_name
FROM products
WHERE LOWER(product_name) LIKE '%queso%';
```

### 8. Orders shipping to France or Belgium
```sql
SELECT
	order_id,
	customer_id,
	ship_country
FROM orders
WHERE 
	ship_country = 'France'
	OR ship_country = 'Belgium';
```

### 9. Orders shipping to any country in Latin America
```sql
SELECT
	order_id,
	customer_id,
	ship_country
FROM orders
WHERE ship_country IN ('Brazil', 'Mexico', 'Argentina', 'Venezuela');
```

### 10. Employees, in order of age
```sql
SELECT
	first_name,
	last_name,
	title,
	birth_date
FROM employees
ORDER BY birth_date;
```

### 11. Showing only the Date with a DateTime field
```sql
SELECT
	first_name,
	last_name,
	title,
	birth_date::date
FROM employees
ORDER BY birth_date;
```

### 12. Employees full name
```sql
SELECT
	first_name,
	last_name,
	(first_name || ' ' || last_name) AS full_name
FROM employees;
```

### 13. OrderDetails amount per line item
```sql
SELECT
	order_id,
	product_id,
	unit_price,
	quantity,
	ROUND((unit_price * quantity)::numeric, 2) total_price
FROM order_details
ORDER BY order_id, product_id;
```

### 14. How many customers?
```sql
SELECT
	COUNT(*) AS "TotalCustomers"
FROM customers;
```

### 15. When was the first order?
```sql
SELECT
	MIN(order_date)
FROM orders;
```

### 16. Countries where there are customers
- One way
```sql
SELECT 
	DISTINCT country
FROM customers
ORDER BY country;
```

- The other way
```
SELECT 
	country
FROM customers
GROUP BY country
ORDER BY country;
```

### 17. Contact titles for customers
```sql
SELECT
	contact_title AS "ContactTitle",
	COUNT(*) AS "TotalContactTitle"
FROM customers
GROUP BY 1
ORDER BY 2 DESC
```

### 18. Products with associated supplier names
```sql
SELECT
	p.product_id,
	p.product_name,
	s.company_name
FROM products p
JOIN suppliers s ON p.supplier_id = s.supplier_id;
ORDER BY 1
```

### 19. Orders and the Shipper that was used
```sql
SELECT
	o.order_id,
	o.order_date,
	s.company_name
FROM orders o
JOIN shippers s ON o.ship_via = s.shipper_id
WHERE o.order_id < 10300
ORDER BY 1;
```
