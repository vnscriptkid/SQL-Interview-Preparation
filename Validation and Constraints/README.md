## :star: 2 ways of adding constraints
- :triangular_flag_on_post: At the time defining table
- :scissors: Update table later
  - Be careful with existing data violating new constraints, that leads to error

## :star: Types of contraints
:one: `NULL` constraint

:two: `UNIQUE` constraint

:three: >, <, >=, <=, =
```sql
    price INTEGER CHECK (price > 0)
```

## :star: Database validation vs Server validation

![image](https://user-images.githubusercontent.com/28957748/121059361-99697780-c7eb-11eb-8264-ddc6f62490e6.png)
