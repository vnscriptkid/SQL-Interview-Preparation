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

```sql
select
	round(100.0 * count(*) filter(where rating = 'NC-17') / count(rating)) as "% NC-17",
	round(100.0 * count(rating) filter(where rating = 'PG') / count(rating)) as "% PG",
	round(100.0 * count(rating) filter(where rating = 'G') / count(rating)) as "% G",
	round(100.0 * count(rating) filter(where rating = 'R') / count(rating)) as "% R",
	round(100.0 * count(rating) filter(where rating = 'PG-13') / count(rating)) as "% PG-13"
from film;
```

### 5.4 Try a few of the different explicit casting operations listed below to get familiar with how casting behaves. Was the behaviour what you expected?

```sql
select int ‘33’;
select int ‘33.3’;
select cast(33.3 as int);
select cast(33.8 as int);
select 33::text;
select ‘hello’::varchar(2);
select cast(35000 as smallint);
select 12.1::numeric(1,1);
```

- second query fails - when you’re using the decorated literal form, the literal must exactly match the target type
- the third and fourth queries when 33.3 and 33.8 are cast to int, both succeed and round up or down to the nearest integer
- casting some literal text to a varchar works, but the result is truncated to as many characters as the varchar can fit
- The final two queries fail - 35,000 does not fit within a smallint and likewise 12.1 does not fit within a numeric(1,1)
