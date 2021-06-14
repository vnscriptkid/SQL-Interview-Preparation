## Install dependencies
- `package.json`
```json
{
  "dependencies": {
    "express": "^4.17.1",
    "node-pg-migrate": "^5.9.0",
    "pg": "^8.6.0"
  }
}
```

## Establish connection

- `index.js`
```javascript
const pg = require("pg");

const pool = new pg.Pool({
  host: "localhost",
  port: 5432,
  database: "socialnetwork",
  user: "postgres",
  password: "123456",
});
```

## Execute a query

```javascript
const { rows } = await pool.query("SELECT * FROM posts;");
rows.forEach(row => console.log(row.id))
```
