### 3.8 Write a query to list all the films with a ‘G’ rating
```sql
SELECT
	title,
	rating
FROM film
WHERE rating = 'G';
```

### 3.9 List all the films longer than 2 hours (note each film has a length in minutes)
```sql
SELECT
	title,
	length
FROM film
WHERE length::float / 60 > 2;
```

### 3.10 Write a query to list all the rentals made before June, 2005
```sql
SELECT
	rental_id,
	rental_date
FROM 
	rental
WHERE
	rental_date < '2005-06-01'
```

### 3.11 In Exercise 3.7, you wrote a query to figure out how many times each film must be rented out to cover its replacement cost. Now write a query to return only those films that must be rented out more than 30 times to cover their replacement cost.
```sql
SELECT
	title,
	rental_rate,
	replacement_cost,
	CEIL(replacement_cost / rental_rate) AS rentals_to_break_even
FROM film
WHERE CEIL(replacement_cost / rental_rate) > 30;
```
