# Window functions
  - A set of rows is termed as a window
  - Components:
    - `PARTITION BY`: optional, split set of rows into subsets, if not defined, entire result set of rows considered as the single partition
    - `ORDER BY`: order of rows in a partition

## :star: LEAD()
- What ❓ Provide access to the next row
- When ❓ Compare value of current row with next row
- How 🔯 
	- Example: Compare the sales of the current year with sales of the next year for each product group
	- ![image](https://user-images.githubusercontent.com/28957748/122888781-5cb18a80-d36c-11eb-9ebe-b1b07a818ba4.png)
	```sql
	SELECT
		year, 
		amount,
		group_id,
		LEAD(amount,1) OVER (
			PARTITION BY group_id
			ORDER BY year
		) next_year_sales
	FROM
		sales;
	```
	- Explainations:
		- The `PARTITION BY` clause distributes rows into product groups (or partitions) specified by group id
		- The `ORDER BY` clause sorts rows in each product group by years in ascending order.
		- The `LEAD()` function returns the sales of the next year from the current year for each product group.

## :star: ROW_NUMBER()
- What? Assigns a sequential integer to each row in a result set
- When? Sort rows in groups
- How? 
  - ![image](https://user-images.githubusercontent.com/28957748/122899803-59bb9780-d376-11eb-8cd3-ee154f5c407e.png)
  - ```sql
    SELECT
      item_id,
      item_name,
      category_id,
      ROW_NUMBER () OVER (
        PARTITION BY category_id
        ORDER BY item_name
      )
    FROM
      items;
    ```
