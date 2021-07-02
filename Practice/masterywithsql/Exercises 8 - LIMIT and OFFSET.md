### 3.27 Write a query to return the 3 most recent payments received
```sql
select
	payment_id,
	payment_date
from payment
order by payment_date desc
limit 3;
```

### 3.28 Return the 4 films with the shortest length that are not R rated. For films with the same length, order them alphabetically
```sql
select
	title,
	length,
	rating
from film
where 
  rating != 'R'
  or rating is null
order by length, title
limit 4;
```

### 3.29 Write a query to return the last 3 payments made in January, 2007
```sql
select
	payment_id,
	amount,
	payment_date
from payment
where 
  payment_date >= '2007-01-01' 
  and payment_date < '2007-02-01'
order by payment_date desc
limit 3;
```

### 3.30 Can you think of a way you could, as in the previous exercise, return the last 3 payments made in January, 2007 but have those same 3 output rows ordered by date ascending? (Don’t spend too long on this…)

