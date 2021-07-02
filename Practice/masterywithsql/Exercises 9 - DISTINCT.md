### 3.31 Write a query to return all the unique ratings films can have, ordered alphabetically (not including NULL)
```sql
select distinct rating
from film
where rating is not null
order by rating;
```

### 3.32 Write a query to help us quickly see if there is any hour of the day that we have never rented a film out on (maybe the staff always head out for lunch?)
```sql
select
	distinct date_part('hour', rental_date)
from rental
order by 1;
```

### 3.33 Write a query to help quickly check whether the same rental rate is used for each rental duration (for example - is the rental rate always 4.99 when the rental duration is 3?)
```sql
select distinct 
	rental_duration,
	rental_rate
from film
order by rental_duration;
```
:star: This query will return all unique combinations of rental_duration and rental_rate => many different rental rates can be used for the same rental duration

### 3.34 Can you explain why the first query below works, but the second one, which simply adds the DISTINCT keyword, doesn’t? (this is quite challenging)
```sql
select first_name
from actor
order by last_name;

select distinct first_name
from actor
order by last_name;
```
In the second query, multiple rows of actors are combined in to a single row due to the use of DISTINCT. For example, there are two actors with the first name ADAM (ADAM HOPPER and ADAM GRANT), however after the SELECT DISTINCT clause has been processed, there is only one row with first name ADAM. Ordering then by last name is undefined - eg. In the case of ADAM, Postgres has no way to know which last name should be used (HOPPER or GRANT?). In general, avoid ordering by columns you haven’t selected and you can sidestep complex situations like this.

