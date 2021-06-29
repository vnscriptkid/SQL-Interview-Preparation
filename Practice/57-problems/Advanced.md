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

## 48. Customer grouping


