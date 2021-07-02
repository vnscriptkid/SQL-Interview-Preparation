### 3.38 We want to give a prize to 5 random customers each month. Write a query that will return 5 random customers each time it is run (you may find a particular math function helpful - make sure to search the documentation!)
```sql
select
	first_name,
	last_name,
	email
from customer
order by random()
limit 5;
```

### 3.39 Give 3 different solutions to list the rentals made in June, 2005. In one solution, use the date_part function. In another, use the BETWEEN keyword. In the third, don’t use either date_part or BETWEEN.
```sql
select
	rental_id,
	rental_date
from rental
where
	date_part('year', rental_date) = 2005
	and date_part('month', rental_date) = 6;
```

```sql
select
	rental_id,
	rental_date
from rental
where
	rental_date between '2005-06-01 00:00:00' and '2005-06-30 24:00:00'
```

```sql
select
	rental_id,
	rental_date
from rental
where rental_date >= '2005-06-01' and rental_date < '2005-07-01';
```

### 3.40 Return the top 5 films for $ per minute (rental_rate / length) of entertainment
```sql
select
	title,
	rental_rate,
	length,
	(rental_rate / length) as per_minute
from film
where
  length is not null
  and length != 0
order by per_minute desc
limit 5;
```

### ⚠️ 3.41 Write a query to list all customers who have a first name containing the letter ‘A’ twice or more
```sql
select first_name
from customer
where first_name ilike '%a%a%';
```

### ⚠️ 3.42 PostgreSQL supports an interesting variation of DISTINCT called DISTINCT ON. Visit the official documentation page and read about DISTINCT ON. See if you can figure out how you would use it in a query to return the most recent rental for each customer
```sql
select 
	distinct on (customer_id) customer_id,
	rental_date
from rental
order by customer_id, rental_date desc;
```

### 3.43 Write a query to list all the customers with an email address but not in the format [first_name].[last_name]@sakilacustomer.org
```sql
select
	first_name,
	last_name,
	email
from customer
where email != (first_name || '.' || last_name || '@sakilacustomer.org');
```
