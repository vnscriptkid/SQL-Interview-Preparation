### 6.1 Write a query to return a list of all the films rented by PETER MENARD showing the most recent first
```sql
select
	r.rental_date,
	f.title
from customer c
inner join rental r on c.customer_id = r.customer_id
inner join inventory i on r.inventory_id = r.inventory_id
inner join film f on i.film_id = f.film_id
where first_name = 'PETER' and last_name = 'MENARD'
order by r.rental_date desc;
```

### 6.2 Write a query to list the full names and contact details for the manager of each store
```sql
select
	store.store_id,
	staff.first_name || ' ' || staff.last_name as Manager,
	email
from store
inner join staff on staff.staff_id = store.manager_staff_id;
```

### 6.3 Write a query to return the top 3 most rented out films and how many times they’ve been rented out
```sql
select
	f.film_id,
	f.title,
	count(*)
from rental r
inner join inventory i on r.inventory_id = i.inventory_id
inner join film f on f.film_id = i.film_id
group by f.film_id
order by 3 desc
limit 3;
```

### 6.4 Write a query to show for each customer how many different (unique) films they’ve rented and how many different (unique) actors they’ve seen in films they’ve rented

### 6.5 Re-write the query below written in the older style of inner joins (which you still encounter surprisingly often online) using the more modern style. 
### Re-write it once using ON to establish the join condition and the second time with USING.
