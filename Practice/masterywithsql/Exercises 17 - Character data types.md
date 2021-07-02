### 5.1 Write a query to print a description of each film’s length as shown in the output below. When a film does not have a length, print: [title] is unknown length

```sql
select
	case
		when length is not null then title  || ' is ' || length || ' minutes'
		else title || ' is unknown length'
	end as length_desc
from film;
```

```sql
select
	title  || ' is ' || coalesce(length || 'minutes', title || ' is unknown length')
from film;
```

### 5.2 You want to play a movie title guessing game with some friends. 
### Write a query to print only the first 3 letters of each film title and then ‘*’ for the rest (The repeat function may come in handy here…)

```sql
select
	substring(title, 1, 3) || repeat('*', length(title) - 3)
from film;
```

```sql
select
	left(title, 3) || repeat('*', length(title) - 3) as Guess
from film;
```
