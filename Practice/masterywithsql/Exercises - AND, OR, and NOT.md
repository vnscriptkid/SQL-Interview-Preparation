### 3.12 Write a query to show all rentals made by the customer with ID 388 in 2005

```sql
SELECT
	rental_id,
	rental_date
FROM
	rental
WHERE 
	customer_id = 388
	AND EXTRACT('Y' FROM rental_date) = 2005;
```

```sql
SELECT
	rental_id,
	rental_date
FROM
	rental
WHERE 
	customer_id = 388
	AND rental_date >= '2005-01-01'
	AND rental_date < '2006-01-01'
```

```sql
SELECT
	rental_id,
	rental_date
FROM
	rental
WHERE 
	customer_id = 388
	AND rental_date BETWEEN '2005-01-01' AND '2005-12-31';
```

### 3.13 We’re trying to list all films with a length of an hour or less. Show two different ways to fix our query below that isn’t working (one using the NOT keyword, and one without)

```sql
select 
	title, rental_duration, length
from 
	film
where 
	not length > 60;
```

```sql
select 
	title, rental_duration, length
from 
	film
where 
	length <= 60;
```

### 3.14 Explain what each of the two queries below are doing and why they generate different results. Which one is probably a mistake and why?
```sql
select title, rating
from film
where rating != ‘G’
and rating != ‘PG’;
```

```sql
select title, rating
from film
where rating != ‘G’
or rating != ‘PG’;
```

### 3.15 Write a single query to show all rentals where the return date is greater than the rental date, or the return date is equal to the rental date, or the return date is less than the rental date. How many rows are returned? Why doesn’t this match the number of rows in the table overall?
```sql
select
	rental_id,
	rental_date,
	return_date
from rental
where
	return_date > rental_date
	OR return_date = rental_date
	OR return_date < rental_date;
```
