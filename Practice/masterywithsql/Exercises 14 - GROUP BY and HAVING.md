### 4.6 List the number of films each actor has appeared in and order the results from most popular to least
```sql
select
	actor_id,
	count(film_id) as num_films
from film_actor
group by actor_id
order by num_films desc;
```

### 4.7 List the customers who have made over 40 rentals
```sql
select
	customer_id
from rental
group by customer_id
having count(*) >= 40;
```

### 4.8 We want to compare how the staff are performing against each other on a month to month basis. 
### So for each month and year, show for each staff member how many payments they handled, the total amount of money they accepted, and the average payment amount
```sql
select
	date_part('year', payment_date) as year,
	date_part('month', payment_date) as month,
	staff_id,
	count(*) num_payments,
	sum(amount) payment_total,
	avg(amount) as avg_payment_amount
from payment
group by staff_id, year, month
order by year, month, staff_id;
```
