### 3.5 Write a query to obtain the length of each customer’s first name 
```sql
SELECT 
  first_name, 
  LENGTH(first_name) 
FROM customer;
```

### 3.6 Write a query to return the initials for each customer
```sql
SELECT 
	first_name, 
	last_name,
	(SUBSTRING(first_name, 1, 1) || SUBSTRING(last_name, 1, 1)) AS initial
FROM customer;
```

```sql
SELECT 
	first_name, 
	last_name,
	LEFT(first_name, 1) || LEFT(last_name, 1) AS initial
FROM customer;
```

### 3.7 Each film has a rental_rate, which is how much money it costs for a customer to rent out the film. Each film also has a replacement_cost, which is how much money the film costs to replace. Write a query to figure out how many times each film must be rented out to cover its replacement cost.
```sql
SELECT
	title,
	rental_rate,
	replacement_cost,
	CEIL(replacement_cost / rental_rate) AS rentals_to_break_even
FROM film;
```
