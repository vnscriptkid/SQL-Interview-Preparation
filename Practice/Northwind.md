#### ❓ :one: Orders shipping to USA or France
```sql
SELECT 
	* 
FROM orders
WHERE 
	ship_country = 'France' 
	OR ship_country = 'USA'
ORDER BY ship_country;
```

#### ❓ :two: Total number of orders shipping to USA or France respectively
```sql
SELECT 
	ship_country,
	COUNT(*)
FROM orders
WHERE 
	ship_country = 'France' 
	OR ship_country = 'USA'
GROUP BY ship_country;
```

#### ❓ :three: Orders shipping to any country within latin america
```sql
SELECT *
FROM orders
WHERE 
	ship_country IN ('Argentina', 'Mexico', 'Brazil', 'Venezuela')
ORDER BY ship_country;
```

#### ❓ :four: Show total price for each order line
```sql
SELECT 
	order_id, 
	product_id,
	unit_price,
	quantity,
	discount,
	(unit_price * quantity - discount) AS total_price
FROM order_details
ORDER BY total_price DESC
LIMIT 5;
```

#### ❓ :five: Find the oldest and latest order date
```sql
SELECT 
	MIN(order_date), 
	MAX(order_date) 
FROM orders;
```

#### ❓ :six: Total products in each category
```sql
SELECT 
	category_name, 
	COUNT(*) AS total_products
FROM products p
INNER JOIN categories c ON c.category_id = p.category_id
GROUP BY 1
ORDER BY 2 DESC;
```

#### ❓ :seven: List products that need re-ordering
```sql
SELECT 
	product_name,
	units_in_stock,
	reorder_level
FROM 
	products
WHERE 
	units_in_stock <= reorder_level
ORDER BY product_name5;
```

#### ❓ :eight: Freight analysis
```sql
-- List top 5 highest freight charges by country
SELECT 
	ship_country,
	AVG(freight)
FROM 
	orders
GROUP BY ship_country
ORDER BY 2 DESC
LIMIT 5;

-- List top 5 highest freight charges by country in year 1997
SELECT 
	ship_country,
	AVG(freight)
FROM 
	orders
WHERE order_date BETWEEN ('1997-01-01') AND ('1997-12-31')
GROUP BY ship_country
ORDER BY 2 DESC
LIMIT 5;

-- List top 5 highest freight charges by country in the latest year
SELECT 
	ship_country,
	AVG(freight)
FROM 
	orders
WHERE EXTRACT('Y' FROM order_date) >= EXTRACT('Y' FROM (SELECT MAX(order_date) FROM orders))
GROUP BY ship_country
ORDER BY 2 DESC
LIMIT 5;
```

#### ❓ :nine: Customers with no order
```sql
SELECT
	*
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.customer_id IS NULL;
```

#### ❓ 🔟 Top customers with their total order amount spent
```sql
SELECT
	c.customer_id,
	c.company_name,
	SUM(d.unit_price * d.quantity - d.discount) AS total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_details d ON d.order_id = o.order_id
GROUP BY 1, 2
ORDER BY 3 DESC;
```

#### ❓ 11. Orders with many lines of items (more than 1)
```sql
SELECT
	order_id,
	COUNT(*)
FROM order_details
GROUP BY order_id
HAVING COUNT(*) >= 2
ORDER BY 2;
```

#### ❓ 12. Orders with double entry line items
```sql
-- List orders with double entry line items
SELECT
	order_id,
	quantity,
	COUNT(*)
FROM order_details
WHERE quantity >= 60
GROUP BY order_id, quantity
HAVING COUNT(*) > 1
ORDER BY COUNT(*);

-- Get details of those items
WITH duplicates AS (
	SELECT
	order_id,
	quantity,
	COUNT(*)
	FROM order_details
	WHERE quantity >= 60
	GROUP BY order_id, quantity
	HAVING COUNT(*) > 1
	ORDER BY COUNT(*)
)

SELECT *
FROM order_details
WHERE order_id IN (
	SELECT order_id FROM duplicates
);
```

#### ❓ 13. Late shipped orders by employees
```sql
-- List late shipped orders
SELECT
	order_id,
	shipped_date,
	required_date,
	employee_id
FROM orders
WHERE shipped_date > required_date;

-- Who has the highest late shipping ratio?
WITH employees_with_late_orders AS (
	SELECT
		employee_id,
		COUNT(*) AS late_orders
	FROM orders
	WHERE shipped_date > required_date
	GROUP BY employee_id
	ORDER BY 2 DESC
),
employees_with_all_orders AS (
	SELECT
		employee_id,
		COUNT(*) AS total_orders
	FROM orders
	GROUP BY employee_id
)

SELECT
	e.employee_id,
	e.first_name,
	e.last_name,
	l.late_orders,
	a.total_orders,
	(l.late_orders::DECIMAL * 100 / a.total_orders ) AS late_ratio
FROM employees e
JOIN employees_with_all_orders a ON e.employee_id = a.employee_id
JOIN employees_with_late_orders l ON e.employee_id = l.employee_id
ORDER BY 6 DESC;
```

#### ❓ 14. Countries with customers or suppliers
```sql
SELECT DISTINCT country FROM customers
UNION
SELECT DISTINCT country FROM suppliers;
```

#### ❓ 15. Countries with customers or suppliers using CTE
```sql
WITH customer_countries AS (
	SELECT DISTINCT country FROM customers
),
supplier_countries AS (
	SELECT DISTINCT country FROM suppliers
)

SELECT
	c.country AS customer_country,
	s.country AS supplier_country
FROM customer_countries c
FULL JOIN supplier_countries s ON c.country = s.country;
```

#### ❓ 16. Customers with multiple orders
```sql
WITH customer_next_order AS (
	SELECT
		customer_id,
		order_date,
		LEAD(order_date, 1) OVER (PARTITION BY customer_id ORDER BY order_date) as next_order_date
	FROM orders
)

SELECT
	customer_id,
	order_date,
	next_order_date,
	(next_order_date - order_date) AS days_between
FROM customer_next_order
WHERE (next_order_date - order_date) <= 4;
```
