## Endpoint testing 

```json
  "scripts": {
    "test": "jest --no-cache"
  },
  "dependencies": {
    "supertest": "^6.1.3"
  }
```

## Test structure

![image](https://user-images.githubusercontent.com/28957748/122227157-d8c94f80-cee0-11eb-9670-73c98f73ac00.png)

## Problems with running tests using Jest

#### :one: Problem 1: Jest doesn't exit as connection's still there

![image](https://user-images.githubusercontent.com/28957748/122227868-7de42800-cee1-11eb-85b6-5728f13e78ca.png)

:point_right:
```js
afterAll(async () => {
  // close the connection
  await context.close();
});
```

#### :two: Problem 2: Dev env and Test env share the same db, making it hard to `assert`

:point_right: Use 2 separate databases: one for dev && one for testing

#### :three: Problem 3: Test env needs migrations too!

![image](https://user-images.githubusercontent.com/28957748/122230550-f5b35200-cee3-11eb-8987-3e69196fe20c.png)

:point_right: 
```console
foo@bar:~$ set DATABASE_URL=postgres://postgres:123456@localhost:5432/socialnetwork-test&&npm run migrate up
```

#### :four: Data returned from db is string

![image](https://user-images.githubusercontent.com/28957748/122231153-71150380-cee4-11eb-9601-362468c96a6a.png)

:point_right:
```js
static async count() {
  const { rows } = await pool.query("SELECT COUNT(*) FROM users;");
  return parseInt(rows[0].count);
}
```

#### :five: First run succeeds but second run fails :confused: 

:point_right: Database is not cleaned up after every test

```js
beforeEach(async () => {
  await context.reset();
});
```

:point_right: Assert on the change only 
```js
expect(finishCount).toEqual(startingCount + 1);
```

#### :six: Issues with parallel running: 

![image](https://user-images.githubusercontent.com/28957748/122234663-3fea0280-cee7-11eb-8e73-f78a4ddf3c53.png)

:point_right: Tell Jest to run tests sequencially (by default jest run tests parallel)

```json
"scripts": {
  "migrate": "node-pg-migrate",
  "start": "nodemon index.js",
  "test": "jest --runInBand"
},
```

![image](https://user-images.githubusercontent.com/28957748/122234519-2648bb00-cee7-11eb-900e-752263ba62ef.png)

:point_right: Wanna run tests parallel ? Isolate tests by schemas

![image](https://user-images.githubusercontent.com/28957748/122237593-a4a65c80-cee9-11eb-8db6-4e8261caf5c4.png)

#### :seven: By default every query runs on `public` schema

![image](https://user-images.githubusercontent.com/28957748/122238077-0b2b7a80-ceea-11eb-9c62-9576fd4c4464.png)

:point_right: For every test, create a random role (user) and schema with the same name.
Then queries are gonna run on that schema :smiley:

```js
let roleName = "a" + randomBytes(4).toString("hex");

await pool.connect(DEFAULT_OPTS);

// Create a new role
await pool.query(
  `CREATE ROLE ${roleName} WITH LOGIN PASSWORD '${roleName}'`
);

// Create a schema
await pool.query(`CREATE SCHEMA ${roleName} AUTHORIZATION ${roleName}`);

// Disconnect from PG
await pool.close();

// Run migrations in new schema
await migrate({
  schema: roleName,
  direction: "up",
  log: () => {},
  noLock: true,
  dir: "migrations",
  databaseUrl: {
    host: "localhost",
    post: 5432,
    database: "socialnetwork-test",
    user: roleName,
    password: roleName,
  },
});

// Connect to PG as newly created role
await pool.connect({
  host: "localhost",
  port: 5432,
  database: "socialnetwork-test",
  user: roleName,
  password: roleName,
});

return new Context(roleName);
```

#### Repetitive setup-code in tests

:point_right: Move all db setup code into one single helper class `Context`
- `src/tests/context.js`
```js
const pool = require("../pool");

beforeEach(async () => {});

class Context {
  constructor(roleName) {
    this.roleName = roleName;
  }

  async reset() {
    // Delete data from tables
  }

  async close() {
    // Close current connection
  }

  static async build() {
    // ...
    return new Context(roleName);
  }
}

module.exports = Context;
```

- `src/tests/routes/users.test.js`
```js
const request = require("supertest");
const buildApp = require("../../app");
const UserRepo = require("../../repos/user-repo");
const Context = require("../context");

let context = null;

beforeAll(async () => {
  context = await Context.build();
});

beforeEach(async () => {
  await context.reset();
});

afterAll(async () => {
  await context.close();
});

test("create a user", async () => {
  // ...
});

```

