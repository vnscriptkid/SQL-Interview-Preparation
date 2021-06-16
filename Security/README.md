## SQL Injections

- What bad might happens?
![image](https://user-images.githubusercontent.com/28957748/121989137-c8708200-cdc5-11eb-9214-8d872a035f45.png)

- Solution 1: Prepared statements (db side)
![image](https://user-images.githubusercontent.com/28957748/121989332-28672880-cdc6-11eb-8228-d44ec993685c.png)
  - :star: PostgreSQL does not support parameters for identifiers. If you need to have dynamic database, schema, table, or column names (e.g. in DDL statements) use pg-format package for handling escaping these values to ensure you do not have SQL injection!
  - ```js
    // Instead
    await pool.query(`CREATE ROLE ${roleName} WITH LOGIN PASSWORD '${roleName}'`);
    
    // Do
    // I: identifier, L: literal value
    await pool.query(format('CREATE ROLE %I WITH LOGIN PASSWORD %L', roleName, roleName));
    ```
