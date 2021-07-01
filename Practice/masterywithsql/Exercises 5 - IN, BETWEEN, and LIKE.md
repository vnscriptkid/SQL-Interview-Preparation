### 3.18 Write a query to return the films with a rating of ‘PG’, ‘G’, or ‘PG-13’
<details>
  <summary>See answer</summary>
  
  ```sql
  select
    title,
    rating
  from film
  where rating in ('G', 'PG', 'PG-13');
  ```
</details>

### 3.19 Write a query equivalent to the one below using BETWEEN.
```sql
select title, length
from film
where length >= 90
and length <= 120;
```
<details>
  <summary>See answer</summary>
  
  ```sql
  select title, length
  from film
  where length between 90 and 120;
  ```
</details>

### 3.20 Write a query to return all film titles that end with the the word “GRAFFITI”
<details>
  <summary>See answer</summary>
  
  ```sql
  select title
  from film
  where title like '%GRAFFITI';
  ```
</details>

### 3.21 In exercise 3.17 you wrote a query to list the films that have a rating that is not ‘G’ or ‘PG’. Re-write this query using NOT IN. Do your results include films with a NULL rating?
<details>
  <summary>See answer</summary>
  
  ```sql
  select title
  from film
  where rating not in ('G', 'PG') or rating is null;
  ```
</details>
:star: `not in` also excludes `null` rating


