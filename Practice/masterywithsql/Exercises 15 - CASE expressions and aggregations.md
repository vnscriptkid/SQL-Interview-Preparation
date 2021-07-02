### ⚠️ 4.9 Write a query to show the number of rentals that were returned within 3 days, the number returned in 3 or more days, and the number never returned 
### (for the logical comparison check you can use the following code snippet to compare against an interval: where return_date - rental_date < interval ‘3 days’)

```sql
select
	sum(case when return_date is not null and return_date - rental_date < interval '3 days' then 1 end) as "lt 3 days",
	sum(case when return_date is not null and return_date - rental_date > interval '3 days' then 1 end) as "gt 3 days",
	sum(case when return_date is null then 1 end) as "never returned"
from rental;
```

### ⚠️ 4.10 Write a query to give counts of the films by their length in groups of 0 - 1hrs, 1 - 2hrs, 2 - 3hrs, and 3hrs+ 
### (note: you might get slightly different numbers if doing inclusive or exclusive grouping - but don’t sweat it!)

```sql
select
	case
		when length >=0 and length < 60 then '0-1hrs'
		when length >= 60 and length < 120 then '1-2hrs'
		when length >= 120 and length < 180 then '2-3hrs'
		else '3hrs+'
	end as len,
	count(*) as count
from film
group by 1
order by 1;
```

```sql
select
	count(*) filter (where return_date - rental_date < interval '3 days') as "lt 3 days",
	count(*) filter (where return_date - rental_date >= interval '3 days') as "gt 3 days",
	count(*) filter (where return_date is null) as "never returned"
from rental;
```
