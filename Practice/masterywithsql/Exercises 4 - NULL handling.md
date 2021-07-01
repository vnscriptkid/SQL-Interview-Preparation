### 3.16 Write a query to list the rentals that haven’t been returned

<details>
  <summary>See answer!</summary>
  
  ```sql
  SELECT
    rental_id,
    return_date
  FROM rental
  WHERE return_date IS NULL;
  ```
</details>

### 3.17 Write a query to list the films that have a rating that is not ‘G’ or ‘PG’
<details>
  <summary>See answer</summary>
  
  ```sql
  SELECT
    title,
    rating
  FROM film
  WHERE 
    rating != 'G' 
    AND rating != 'PG' 
    OR rating IS NULL;
  ```
</details>

:star: __Postgres excludes NULL in WHERE__

https://stackoverflow.com/questions/19974472/postgres-excludes-null-in-where-id-int-query
