## :star: Types of subqueries
:one: In `SELECT`
  - Resolve to __single value__ (scalar)

:two: In `FROM`
  - Any type, as long as outer SELECTs/WHEREs/... are compatible
  - Must have an __alias__

:three: In `JOIN`
  - Any type, as long as it's compatible with `ON` clause
  - Must have an __alias__

:four: In `WHERE`
  - Resolve to either __single value__ or __single column__

![image](https://user-images.githubusercontent.com/28957748/121020953-0d475800-c7cb-11eb-924b-ab5ef8061595.png)


## :star: Shape of a query
![image](https://user-images.githubusercontent.com/28957748/121020671-c9545300-c7ca-11eb-94d6-1c4549059e81.png)


## :star: Collerated subqueries
:question: Show the name, department, and price of the __most expensive product__ in __each department__

![image](https://user-images.githubusercontent.com/28957748/121034322-21915200-c7d7-11eb-8200-5b0315130ba8.png)

:100::smiley::100:
```sql
SELECT name, department, price
FROM products AS p1
WHERE p1.price = (
  SELECT MAX(price)
  FROM products AS p2
  WHERE p2.department = p1.department
)
```

## :star: `SELECT` without `FROM`
:question: 

![image](https://user-images.githubusercontent.com/28957748/121035841-784b5b80-c7d8-11eb-95ac-f1514bf80769.png)

![image](https://user-images.githubusercontent.com/28957748/121035738-5f42aa80-c7d8-11eb-8f7a-ba453fd24e46.png)

:100::smiley::100:
```sql
SELECT 
    (SELECT MAX(price) FROM phones) AS max_price,
    (SELECT MIN(price) FROM phones) AS min_price,
    (SELECT AVG(price) FROM phones) AS avg_price; 
```
