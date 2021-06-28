## 20. Categories, and the total products in each category
```sql
SELECT
	category_name,
	COUNT(*) AS "TotalProducts"
FROM products p
JOIN categories c ON p.category_id = c.category_id
GROUP BY category_name
ORDER BY 2 DESC;
```

## 21. Total customers per country/city
```sql
SELECT
	country,
	city,
	COUNT(*) AS TotalCustomer
FROM customers
GROUP BY country, city
ORDER BY 3 DESC;
```

## 22. Products that need reordering
```sql
SELECT
	product_id,
	product_name,
	units_in_stock,
	reorder_level
FROM products
WHERE units_in_stock < reorder_level
ORDER BY product_id;
```

## 23. Products that need reordering, continued
```sql
SELECT
	product_id,
	product_name,
	units_in_stock,
	units_on_order,
	reorder_level,
	discontinued
FROM products
WHERE 
	(units_in_stock + units_on_order) <= reorder_level
	AND discontinued = 0	
ORDER BY product_id;
```

## 24. Customer list by region
```sql
SELECT
	customer_id,
	company_name,
	region,
	CASE
		WHEN region IS NULL THEN 1
		ELSE 0
	END
FROM customers
ORDER BY 4, 3, 1;
```

## 25. High freight charges
```sql
SELECT
	ship_country,
	ROUND(AVG(freight)::numeric, 2) AS AverageFreight
FROM orders
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3;  
```

## 26. High freight charges - 2015
```sql
SELECT
	ship_country,
	ROUND(AVG(freight)::numeric, 2) AS AverageFreight
FROM orders
WHERE EXTRACT('Y' FROM shipped_date) = 2015
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3;
```

## 27. High freight charges with between
```sql
SELECT
	ship_country,
	ROUND(AVG(freight)::numeric, 2) AS AverageFreight
FROM orders
WHERE shipped_date BETWEEN '1996-01-01' AND '1996-12-31'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3;
```

## 28. High freight charges - last year
```sql
SELECT
	ship_country,
	ROUND(AVG(freight)::numeric, 2) AS AverageFreight
FROM orders
WHERE EXTRACT('Y' FROM shipped_date) = (SELECT MAX(EXTRACT('Y' FROM shipped_date)) FROM orders) - 1
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3;
```

## 29. Inventory list
```sql
SELECT
	e.employee_id,
	e.last_name,
	o.order_id,
	p.product_name,
	d.quantity
FROM products p
JOIN order_details d ON d.product_id = p.product_id
JOIN orders o ON d.order_id = o.order_id
JOIN employees e ON e.employee_id = o.employee_id
ORDER BY o.order_id, p.product_id;
```

## 30. Customers with no orders
```sql
SELECT
	c.customer_id AS "Customers_CustomerID",
	o.customer_id AS "Orders_CustomerID"
FROM orders o
RIGHT JOIN customers c ON o.customer_id = c.customer_id
WHERE o.customer_id IS NULL;
```

## 31. Customers with no orders for EmployeeID 4
