### 4.1 Write a query to return the total count of customers in the customer table and the count of how many customers provided an email address
```sql
select 
	count(*) as "# customers",
	count(email) as "# customers with email"
from customer;
```
:star: COUNT(expression) will return the number of rows where expression is not NULL

### 4.2 Building on the previous exercise, now return an additional result showing the percentage of customers with an email address 
### (as a helpful hint, if you’re getting 0 try multiplying the fraction by 100.0 - we’ll examine why this is necessary in an upcoming chapter on data types)
```sql
select 
	count(*) as "# customers",
	count(email) as "# customers with email",
	count(email)::float * 100 / count(*) as "% with email"
from customer;
```

### 4.3 Write a query to return the number of distinct customers who have made payments
```sql
select
	count(distinct customer_id)
from payment;
```

### 4.4 What is the average length of time films are rented out for
```sql
select
	avg(return_date - rental_date) as avg_rental_duration
from rental
where return_date is not null;
```

### 4.5 Write a query to return the sum total of all payment amounts received
```sql
select
	sum(amount)
from payment;
```
