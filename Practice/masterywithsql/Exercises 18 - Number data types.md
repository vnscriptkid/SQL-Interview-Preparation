### 5.3 Write a query to list the percentage of films that are rated NC-17, G, PG, PG-13, NC-17, and R, rounded to the nearest integer.
```sql
select
	round(100.0 * count(case when rating = 'NC-17' then 1 end) / count(*)) as "% NC-17",
	round(100.0 * count(case when rating = 'PG' then 1 end) / count(*)) as "% PG",
	round(100.0 * count(case when rating = 'G' then 1 end) / count(*)) as "% G",
	round(100.0 * count(case when rating = 'R' then 1 end) / count(*)) as "% R",
	round(100.0 * count(case when rating = 'PG-13' then 1 end) / count(*)) as "% PG-13"
from film;
```

### 5.4 Try a few of the different explicit casting operations listed below to get familiar with how casting behaves. Was the behaviour what you expected?
