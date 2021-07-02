### 3.26 Fix the query below, which we wanted to use to list all the rentals that happened after 10pm at night
```sql
select rental_id, date_part(‘hour’, rental_date) as “rental hour”
from rental
where date_part(‘hour’, rental_date) >= 22;
```
```sql
select 
	rental_id, 
	date_part('hour', rental_date) as "rental hour"
from rental
where date_part('hour', rental_date) >= 22;
```
