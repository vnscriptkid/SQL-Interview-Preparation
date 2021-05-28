## 1. Database, SQL, Tables, Rows, Columns
- Database is a software, that allows storing data and retrieving data in a structured way.
- SQL is the kind of language to communicate with Database, interacting with data. Create, Update, Delete are most common operations.
- Data in database is organized in tables. A table is a set of related data. For example: `Users` table contains users's information ...
- Columns form shape of table, telling us what kind of information table contains. For example: `Users` table will probably have: `name`, `age`, `job` ... Columns are defined at the time creating table, but can be changed later. Each column has a type like: `varchar`, `int` ...
- Rows are actual data of table. One row corresponds to one record of data. 1 row of `Users` table would be a record about 1 specific user. 

## 2. Basic syntax
- CREATE TABLE
- INSERT INTO
- SELECT * FROM
- Calculated columns, renaming columns `AS` (aliasing)
- String operators and Functions
    - Join 2 strings: `||` or `CONCAT`
    - Casing: `LOWER()`, `UPPER()`
    - Num of chars: `LENGTH()`

## 3. Filtering Rows, Delete, Update
- `WHERE`
- Compound `WHERE`
    - `=`, `>`, `<`, `>=`, `<=`, `<>`, `!=`
    - `BETWEEN`, `IN`, `NOT IN`
- `WHERE` with List
    - `WHERE manufacturer IN ('Apple', 'Samsung');`
    - `WHERE manufacturer = 'Apple' OR manufacturer = 'Samsung';`
- Calculations in `WHERE`: :star: __Can't refer to alias in WHERE__ :star:
  ```sql
  SELECT name, price * units_sold AS total_revenue 
  FROM phones 
  WHERE price * units_sold > 1000000
  ```
- Update
  ```sql
  UPDATE phones
  SET units_sold = 8543
  WHERE name = 'N8';
  ```
- Delete
  ```sql
  DELETE FROM phones 
  WHERE manufacturer = 'Samsung';
  ```
