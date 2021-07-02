### 3.35 Write a query to return an ordered list of distinct ratings for films in our films table along with their descriptions (you will have to type in the descriptions yourself)
```sql
select
	distinct rating,
	case rating
		when 'G' then 'General'
		when 'PG' then 'Parental Guidance Recommended'
		when 'PG-13' then 'Parents Strongly Cautioned'
		when 'R' then 'Restricted'
		when 'NC-17' then 'Adults Only'
		else 'Others'
	end as "Rating description"
from film
where rating is not null
order by 1;
```

### 3.36 Write a query to output ‘Returned’ for returned rentals and ‘Not Returned’ for rentals that haven’t been returned. Order the output to show those not returned first.
```sql
select
	rental_id,
	rental_date,
	return_date,
	case
		when return_date is null then 'Not Returned'
		else 'Returned'
	end as return_status
from rental
order by return_status;
```

### 3.37 Imagine you were asked to write a query to populate a ‘country picker’ for some internal company dashboard. Write a query to return the countries in alphabetical order, but also with the twist that the first 3 countries in the list must be 1) Australia 2) United Kingdom 3) United States and then normal alphabetical order after that (maybe you want them first because, for example, most of your customers come from these countries)
```sql
select
	country
from
	country
order by
	case country
		when 'Australia' then 1
		when 'United Kingdom' then 2
		when 'United States' then 3
		else 4
	end, country
```
