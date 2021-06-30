## 32. High-value customers
```sql
SELECT
	o.customer_id,
	c.company_name,
	o.order_id,
	ROUND(SUM(d.unit_price * d.quantity)::numeric, 2) AS total
FROM orders o
JOIN order_details d ON o.order_id = d.order_id
JOIN customers c ON o.customer_id = c.customer_id
WHERE EXTRACT('Y' FROM o.order_date) = 1997
GROUP BY o.customer_id, c.company_name, o.order_id
HAVING SUM(d.unit_price * d.quantity) >= 10000
ORDER BY 4;
```

## 33. High-value customers - total orders
```sql
SELECT
	o.customer_id,
	c.company_name,
	ROUND(SUM(d.unit_price * d.quantity)::numeric, 2) AS total
FROM orders o
JOIN order_details d ON o.order_id = d.order_id
JOIN customers c ON o.customer_id = c.customer_id
WHERE EXTRACT('Y' FROM o.order_date) = 1997
GROUP BY o.customer_id, c.company_name
HAVING SUM(d.unit_price * d.quantity) >= 15000
ORDER BY 3;
```

## 34. High-value customers - with discount
```sql
SELECT
	o.customer_id,
	c.company_name, 
	ROUND(SUM(d.unit_price * d.quantity)::numeric, 2) AS total_without_discount,
	ROUND(SUM(d.unit_price * d.quantity * (1 - d.discount))::numeric, 2) AS total_with_discount
FROM orders o
JOIN order_details d ON o.order_id = d.order_id
JOIN customers c ON o.customer_id = c.customer_id
WHERE EXTRACT('Y' FROM o.order_date) = 1997
GROUP BY o.customer_id, c.company_name
HAVING SUM(d.unit_price * d.quantity) >= 15000
ORDER BY 3;
```

## 35. Month-end orders
```sql
SELECT
	employee_id,
	order_id,
	order_date
FROM orders
WHERE order_date = (
	SELECT (date_trunc('month', order_date::date) + interval '1 month' - interval '1 day')::date	
)
ORDER BY employee_id, order_id;
```

## 36. Orders with many line items
```sql
SELECT
	order_id,
	COUNT(*) AS "TotalOrderDetails"
FROM order_details
GROUP BY order_id
ORDER BY 2 DESC
LIMIT 10;
```

## ⚠️ 37. Orders - random assortment
```sql
SELECT order_id
FROM orders
ORDER BY random() LIMIT (
	SELECT 
		CEIL(COUNT(*) * .02) AS "2% orders"
	FROM orders
);
```

## ⚠️ 38. Orders - accidental double-entry
```sql
SELECT
	d.order_id,
	COUNT(*)
FROM orders o
JOIN order_details d ON o.order_id = d.order_id
WHERE 
	o.employee_id = (
		SELECT
			employee_id
		FROM employees
		WHERE
			first_name = 'Janet'
			AND last_name = 'Leverling'
	)
	AND d.quantity >= 60
GROUP BY d.order_id, d.quantity;
```

## ⚠️ 39. Orders - accidental double-entry details

## ⚠️ 40. Orders - accidental double-entry details, derived table

## 41. Late orders
```sql
SELECT
	order_id,
	order_date,
	required_date,
	shipped_date
FROM orders
WHERE shipped_date > required_date;
```

## 42. Late orders - which employees?
```sql
SELECT
	o.employee_id,
	e.last_name,
	COUNT(*)
FROM orders o
JOIN employees e ON e.employee_id = o.employee_id
WHERE o.shipped_date > o.required_date
GROUP BY 1, 2
ORDER BY 3 DESC;
```

## 43. Late orders vs. total orders
```sql
WITH late_orders AS (
	SELECT
		o.employee_id,
		COUNT(*)
	FROM orders o
	JOIN employees e ON e.employee_id = o.employee_id
	WHERE o.shipped_date > o.required_date
	GROUP BY o.employee_id
),
total_orders AS (
	SELECT
		o.employee_id,
		COUNT(*)
	FROM orders o
	JOIN employees e ON e.employee_id = o.employee_id
	GROUP BY o.employee_id
)
SELECT 
	e.employee_id,
	e.last_name,
	t.count AS "AllOrders",
	l.count AS "LateOrders"
FROM late_orders l
JOIN total_orders t ON t.employee_id = l.employee_id
JOIN employees e ON e.employee_id = l.employee_id;
```

## 44. Late orders vs. total orders - missing employee

## 45. Late orders vs. total orders - fix null

## 46. Late orders vs. total orders - percentage
```sql
WITH late_orders AS (
	SELECT
		o.employee_id,
		COUNT(*)
	FROM orders o
	JOIN employees e ON e.employee_id = o.employee_id
	WHERE o.shipped_date > o.required_date
	GROUP BY o.employee_id
),
total_orders AS (
	SELECT
		o.employee_id,
		COUNT(*)
	FROM orders o
	JOIN employees e ON e.employee_id = o.employee_id
	GROUP BY o.employee_id
)
SELECT 
	e.employee_id,
	e.last_name,
	t.count AS "AllOrders",
	l.count AS "LateOrders",
	(l.count::float / t.count) AS "PercentLateOrders"
FROM late_orders l
JOIN total_orders t ON t.employee_id = l.employee_id
JOIN employees e ON e.employee_id = l.employee_id;
```

## 47. Late orders vs. total orders - fix decimal
```sql
WITH late_orders AS (
	SELECT
		o.employee_id,
		COUNT(*)
	FROM orders o
	JOIN employees e ON e.employee_id = o.employee_id
	WHERE o.shipped_date > o.required_date
	GROUP BY o.employee_id
),
total_orders AS (
	SELECT
		o.employee_id,
		COUNT(*)
	FROM orders o
	JOIN employees e ON e.employee_id = o.employee_id
	GROUP BY o.employee_id
)
SELECT 
	e.employee_id,
	e.last_name,
	t.count AS "AllOrders",
	l.count AS "LateOrders",
	ROUND(l.count::numeric / t.count, 2) AS "PercentLateOrders"
FROM late_orders l
JOIN total_orders t ON t.employee_id = l.employee_id
JOIN employees e ON e.employee_id = l.employee_id;
```

## ⚠️ 48. Customer grouping
```sql
WITH total_amount_customers AS (
	SELECT
		o.customer_id,
		ROUND(SUM(unit_price * quantity)::numeric, 2) AS total
	FROM orders o
	JOIN order_details d ON o.order_id = d.order_id
	WHERE order_date BETWEEN '1996-01-01' AND '1996-12-31'
	GROUP BY o.customer_id
)
SELECT 
	c.customer_id,
	c.company_name,
	t.total,
	CASE
		WHEN t.total >= 0 AND t.total < 1000 THEN 'Low'
		WHEN t.total >= 1000 AND t.total < 5000 THEN 'Medium'
		WHEN t.total >= 5000 AND t.total < 10000 THEN 'High'
		ELSE 'Very High'
	END "CustomerGroup"
FROM total_amount_customers t
JOIN customers c ON t.customer_id = c.customer_id
ORDER BY 1;
```

## 49. Customer grouping - fix null

## 50. Customer grouping with percentage
```sql
WITH total_amount_customers AS (
	SELECT
		o.customer_id,
		ROUND(SUM(unit_price * quantity)::numeric, 2) AS total
	FROM orders o
	JOIN order_details d ON o.order_id = d.order_id
	WHERE order_date BETWEEN '1996-01-01' AND '1996-12-31'
	GROUP BY o.customer_id
)
SELECT 
	CASE
		WHEN t.total >= 0 AND t.total < 1000 THEN 'Low'
		WHEN t.total >= 1000 AND t.total < 5000 THEN 'Medium'
		WHEN t.total >= 5000 AND t.total < 10000 THEN 'High'
		ELSE 'Very High'
	END "CustomerGroup",
	COUNT(*) AS "TotalInGroup",
	COUNT(*)::float / (SELECT COUNT(*) FROM total_amount_customers) AS "PercentageInGroup"
FROM total_amount_customers t
JOIN customers c ON t.customer_id = c.customer_id
GROUP BY 1;
```

## 51. Customer grouping - flexible

## 52. Countries with suppliers or customers
```sql
SELECT DISTINCT country FROM customers
UNION
SELECT DISTINCT country FROM suppliers;
```

## 53. Countries with suppliers or customers, version 2
```sql
WITH customer_countries AS (
	SELECT DISTINCT country FROM customers
),
supplier_countries AS (
	SELECT DISTINCT country FROM suppliers
)
SELECT
	s.country AS "SupplierCountry",
	c.country AS "CustomerCountry"
FROM customer_countries c
FULL JOIN supplier_countries s ON c.country = s.country; 
```

## 54. Countries with suppliers or customers, version 2
```sql
WITH customers_by_country AS (
	SELECT
		country,
		COUNT(*)
	FROM customers
	GROUP BY country
), 
suppliers_by_countries AS (
	SELECT
		country,
		COUNT(*)
	FROM suppliers
	GROUP BY country
)
SELECT
	COALESCE(c.country, s.country) AS country,
	COALESCE(c.count, 0) AS "TotalCustomers",
	COALESCE(s.count, 0) AS "TotalSuppliers"
FROM customers_by_country c
FULL JOIN suppliers_by_countries s ON c.country = s.country;
```

## 55. ⚠️ First order in each country
```sql
WITH orders_by_countries AS (
	SELECT
		ship_country,
		customer_id,
		order_id,
		order_date,
		ROW_NUMBER() OVER (PARTITION BY ship_country ORDER BY order_date)
	FROM orders o
	ORDER BY ship_country, order_date
)
SELECT 
	ship_country,
	customer_id,
	order_id,
	order_date
FROM orders_by_countries
WHERE row_number = 1;
```

## 56. Customers with multiple orders in 5 day period
```sql
SELECT
	o1.customer_id,
	o1.order_id AS "InitialOrderId",
	o1.order_date AS "InitialOrderDate",
	o2.order_id AS "NextOrderId",
	o2.order_date AS "NextOrderDate",
	(o2.order_date - o1.order_date) AS "DaysBetween"
FROM orders o1
JOIN orders o2 ON o1.customer_id = o2.customer_id
WHERE 
	o1.order_date < o2.order_date
	AND o2.order_date - o1.order_date <= 5
ORDER BY o1.customer_id;
```

## ⚠️ 57. Customers with multiple orders in 5 day period, version 2
```sql
WITH next_orders AS (
	SELECT
		customer_id,
		order_date,
		LEAD(order_date, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_order_date
	FROM orders
)
SELECT
	customer_id,
	order_date,
	next_order_date,
	(next_order_date - order_date) AS days_between
FROM next_orders
WHERE 
	next_order_date IS NOT NULL
	AND (next_order_date - order_date) <= 5;
```

