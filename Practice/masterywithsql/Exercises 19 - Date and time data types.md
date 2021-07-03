

### ⚠️ 5.5 Show 3 different ways to input the timestamptz representing 4th March, 2019 at 3:30pm in New York, USA
```sql
select
timestamptz '2019-03-04 15:30 EST',
timestamptz '2019-03-04 03:30PM -5',
timestamptz '2019-03-04 03:30PM America/New_York';
```

#### Run the following queries to see the full list of timezones available in Postgres.
```sql
select *
from pg_timezone_names;

select *
from pg_timezone_abbrevs;
```

#### 5.6 The rental duration in the film table is currently stored as an integer, representing the number of days. 
### Write a query to return this as an interval instead and then add one day to the duration
```sql
select
	title,
	rental_duration * '1 day'::interval as duration,
	rental_duration * '1 day'::interval + '1 day' as "duration+1"
from film;
```

```sql
title,
cast(rental_duration || ’ days’ as interval) as duration,
cast(rental_duration || ’ days’ as interval) + interval ‘1 day’ as “duration + 1”
from film;
```

### 5.7 You have a theory that certain hours of the day might be busiest for rentals. 
### To investigate this write a query to list out for all time the the number of rentals made during each hour of the day
```sql
select
	date_part('hour', rental_date) as "hr",
	count(rental_date)
from rental
group by 1
order by 1;
```

### If you wanted to aggregate payments received by year and month you could write a query as below using the date_part function. 
### Try and simplify this query by instead using the date_trunc function to achieve effectively the same result (ignoring the slight difference in date presentation)

```sql
select
date_part(‘year’, payment_date) as “year”,
date_part(‘month’, payment_date) as “month”,
sum(amount) as total
from payment
group by “year”, “month”
order by “year”, “month”;
```

```sql
select
	date_trunc('month', payment_date) as month,
	sum(amount)
from payment
group by 1
order by 1;
```
