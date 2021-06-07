## :star: 3 Notes on UNION
- The number of columns between 2 sets must be the same.
- The names of columns between 2 sets must be the same.
- The data types of 2 columns with same name must be compatible.

![image](https://dotnettutorials.net/wp-content/uploads/2018/07/UNION-and-UNION-ALL-Operators-in-SQL-Server.png)

## :star: UNION vs UNION ALL
- UNION: duplicated rows are removed
- UNION ALL: duplicated rows remain
![image](https://user-images.githubusercontent.com/28957748/121007855-812e3400-c7bc-11eb-975f-8c426b55b3a7.png)

## :star: Visualize `INTERSECT` and `EXCEPT`
![image](https://sanori.github.io/2019/08/Compare-Two-Tables-in-SQL/except-intersect-SQL.png)

## :star: Set operators's definitions

![image](https://user-images.githubusercontent.com/28957748/121013159-b0e03a80-c7c2-11eb-8b70-273e17ad4215.png)

## :star: Example

![image](https://user-images.githubusercontent.com/28957748/121013379-f4d33f80-c7c2-11eb-96d7-e33efa93116f.png)

:100::smiley::100:

```sql
SELECT * FROM products
ORDER BY price DESC
LIMIT 4
UNION
SELECT * FROM products
ORDER BY price / weight DESC
LIMIT 4
```
