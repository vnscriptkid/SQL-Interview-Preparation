### 4.11 Explain why in the following query we obtain two different results for the average film length. Which one is correct?
```sql
select
1.0 * sum(length) / count(*) as avg1,
1.0 * avg(length) as avg2
from film;
```

avg2 is correct as `count(*)` doesn't count on `null` value, whereas `avg(length)` excludes films where `length` is `null`

### 4.12 Write a query to return the average rental duration for each customer in descending order

```sql
select
	customer_id,
	avg(return_date - rental_date) as avg_rent_duration
from rental
group by customer_id
order by 2 desc;
```

### 4.13 Return a list of customer where all payments they’ve made have been over $2 (lookup the bool_and aggregate function which will be useful here)
```sql
select
	customer_id
from payment
group by customer_id
having bool_and(amount > 2)
```
:star: `bool_and` combines multiple rows in to a single output by performing a logical AND between each input expression

### 4.14 As a final fun finish to this chapter, run the following query to see a cool way you can generate ascii histogram charts. 
### Look up the repeat function (you’ll find it under ‘String Functions and Operators’) to see how it works and change the output character…and don’t worry, 
### I’ll explain the ::int bit in the next chapter!
```sql
select rating, repeat('*', (count(*) / 10)::int) as "count/10"
from film
where rating is not null
group by rating;
```
