## 1. Group
- Group by 1 column or by multiple columns
- Can't select ungrouped columns

## 2. Aggregate
- Reduce multiple values into a single value (:star:MMACS)
  - `MIN`
  - `MAX`
  - `AVG`
  - `COUNT` :star: doesn't count on `NULL` column :point_right: `COUNT(*)`
  - `SUM`

## 3. Group then Aggregate
![image](https://learnsql.com/blog/group-by-in-sql-explained/GROUP_BY-avg.jpg)

## 4. Grouping with JOIN
- Print names of authors and the number of books they have authored
  
<table>
<tr>
  <th>authors</th>
  <th>books</th>
</tr>
<tr>
  <td>

|id|name|
|--|--|
|1| JK Rowling |
|2| Stephen King |
|3| Agatha Christie |
|4| Dr Seuss |

  </td>
  <td>
  
|id|title|author_id|
|--|--|--|
|1| Chamber of Secrets |1|
|2| Prisoner of Azkaban |1|
|3| The Dark Tower |2|
|4| Murder at the Links |3|
|5| Affair at Styles |3|
|6| Cat in the Hat |4|

  </td>
</tr>
</table>

```sql
SELECT name, COUNT(*)
FROM books
JOIN authors ON books.author_id = authors.id
GROUP BY authors.id, name
```

## 5. Filtering groups with HAVING
  - `HAVING` vs `WHERE`
    - `WHERE` filters __rows__
    - `HAVING` works on __groups__
  - Can't refer to alias in `HAVING` ⭐ 
  ```sql
  SELECT manufacturer, SUM(price * units_sold) AS total_revenue
  FROM phones
  GROUP BY manufacturer
  HAVING SUM(price * units_sold) > 2000000
  ```
