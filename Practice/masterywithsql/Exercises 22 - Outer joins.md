### 6.6 Write a query to list the films that are not in stock in any of the stores
<details>
  <summary>See answer</summary>
  
  ```sql
  select 
	f.title
  from film f
  left join inventory i on f.film_id = i.film_id
  where i.film_id is null;
  ```
</details>

:star: `LEFT JOIN` and `LEFT OUTER JOIN` are equivalent.


### 6.7 Write a query to return a count of how many of each film we have in our inventory (include all films). 
### Order the output showing the lowest in-stock first so we know to buy more!

<details>
  <summary>See answer</summary>
  
  ```sql
  select
    f.title,
    count(i.inventory_id)
  from film f
  left outer join inventory i on f.film_id = i.film_id
  group by f.film_id, f.title
  order by count(i.film_id);
  ```
</details>
