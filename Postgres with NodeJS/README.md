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

## Workflow
- `package.json`
```json
"scripts": {
  "migrate": "node-pg-migrate",
  "start": "nodemon index.js",
  "test": "jest --no-cache"
},
```

- Fast endpoint testing using `REST Client` VSCode extension (Postman alternative), in `request.http`:
```json
### Get all users
http://localhost:8000/users

### Create a new user
POST http://localhost:8000/users HTTP/1.1
Content-Type: application/json
{
    "username": "linh",
    "bio": "i am a designer"
}
```


## Execute a query

```javascript
const { rows } = await pool.query("SELECT * FROM posts;");
rows.forEach(row => console.log(row.id))
```

## App factory pattern for testing
- `src/app.js`
```javascript
const express = require("express");
const usersRouter = require("./routes/users");

module.exports = () => {
  const app = express();

  app.use(express.json());

  app.use(usersRouter);

  return app;
};
```

- `src/routes/users.js`
```javascript
const express = require("express");
const router = express.Router();

router.get("/users", async (req, res) => {
  // get all users
});
```

## Pool vs Client

## Customizable Pool for testing
`src/pool.js`
```javascript
const pg = require("pg");

class Pool {
  constructor() {
    this._pool = null;
  }

  connect(options) {
    this._pool = new pg.Pool(options);
    return this._pool.query(`SELECT 1 + 1;`);
  }

  close() {
    return this._pool.end();
  }

  query(sql, params) {
    return this._pool.query(sql, params);
  }
}

module.exports = new Pool();
```
## Establish db connection at app entrance

- `index.js`
```javascript
const app = require("./src/app.js");
const pool = require("./src/pool.js");

pool
  .connect({
    host: "localhost",
    port: 5432,
    database: "socialnetwork",
    user: "postgres",
    password: "123456",
  })
  .then(() => {
    app().listen(8000, () => console.log("Server is listening on port 8000"));
  })
  .catch((err) => console.error(err));

```

## Repository pattern
- `src/repos/user-repo.js`
```javascript
const pool = require("../pool");

class UserRepo {
  static async find() {
    const { rows } = await pool.query("SELECT * FROM users;");
    return rows;
  }

  static async findById(id) {}

  static async insert({ username, bio }) {}

  static async update(id, username, bio) {}

  static async delete(id) {}

  static async count() {}
}

module.exports = UserRepo;
```

- `src/routes/users.js`
```javascript
const express = require("express");
const router = express.Router();
const UserRepo = require("../repos/user-repo");

router.get("/users", async (req, res) => {
  const users = await UserRepo.find();

  res.send(users);
});
```

## Camelcase column's name
- `repos/utils/to-camel-case.js`
```javascript
module.exports = (rows) => {
  return rows.map((row) => {
    const output = {};

    for (let key in row) {
      const camelCaseKey = key.replace(/[-_][a-z]/gi, ($1) =>
        $1.replace("_", "").toUpperCase()
      );

      output[camelCaseKey] = row[key];
    }

    return output;
  });
};
```

- `src/repos/user-repo.js`
```javascript
const pool = require("../pool");
const toCamelCase = require("./utils/to-camel-case");

class UserRepo {
  static async find() {
    const { rows } = await pool.query("SELECT * FROM users;");
    return toCamelCase(rows);
  }
}
```

## Mitigate SQL injection issue with Prepared statements
:shit: :shit: :shit:
```javascript
const { rows } = await pool.query(`SELECT * FROM users WHERE id = ${id}`);
```

:muscle: :smiley: :muscle:
```javascript
const { rows } = await pool.query("SELECT * FROM users WHERE id = $1", [ id ]);
```

## CRUD operations
#### :one: CREATE
```javascript
// src/repos/user-repo.js
const { rows } = await pool.query(
  "INSERT INTO users (username, bio) VALUES ($1, $2) RETURNING *;",
  [username, bio]
);
```
#### :two: READ
```javascript
// src/repos/user-repo.js
const { rows } = await pool.query("SELECT * FROM users WHERE id = $1", [ id ]);
```
#### :three: UPDATE
```javascript
// src/repos/user-repo.js
const { rows } = await pool.query(
  "UPDATE users SET username=$1, bio=$2 WHERE id=$3 RETURNING *;",
  [username, bio, id]
);
```
#### :four: DELETE
```javascript
// src/repos/user-repo.js
const { rows } = await pool.query(
  "DELETE FROM users WHERE id=$1 RETURNING *;",
  [id]
);
```




