## Basics
- Sort by:
  - alphabet 'a' < 'b' < 'c'
  - numbers: 1 < 5 < 100
- Direction
  - `ASC` by default
  - `DESC`
- Compound sorting:
  ```sql
  SELECT * FROM persons
  ORDER BY height, weight
  ```
- `OFFSET` && `LIMIT`
  - Convention :one: `LIMIT` :two: `OFFSET`
  - :green_book: Show the names of __only__ the __second__ and __third most expensive__ phones

  |name|manufacturer|price|units_sold|
  |--|--|--|--|
  |N1280|Nokia|199|1925|
  |Iphone4|Apple|399|9436|
  |Galaxy S|Samsung|299|2359|
  
  :100: :smiley: :100:
  ```sql
  SELECT name
  FROM phones
  ORDER BY price DESC
  LIMIT 2
  OFFSET 1
  ```
  
