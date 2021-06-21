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
