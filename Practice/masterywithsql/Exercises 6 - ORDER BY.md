### 3.22 Write a query the list all the customers with an email address. Order the customers by last name descending
```sql
select
	first_name,
	last_name
from customer
where email is not null
order by last_name desc;
```

### 3.23 Write a query to list the country id’s and cities from the city table, first ordered by country id ascending, then by city alphabetically.
```sql
select
	country_id,
	city
from city
order by country_id, city;
```

### 3.24 Write a query to list actors ordered by the length of their full name ("[first_name] [last_name]") descending.
```sql
select
	(first_name || ' ' || last_name) as full_name,
	length(first_name || ' ' || last_name) as len
from actor
order by len desc;
```

### 3.25 Describe the difference between ORDER BY x, y DESC and ORDER BY x DESC, y DESC (where x and y are columns in some imaginary table you’re querying)
- `ORDER BY x, y DESC`: order by x asc, then order by y desc
- `ORDER BY x DESC, y DESC`: order by x desc, then order by y desc
