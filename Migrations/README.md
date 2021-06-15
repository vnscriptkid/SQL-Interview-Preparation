## :star: Problems

![image](https://user-images.githubusercontent.com/28957748/121934759-e7ddbf80-cd71-11eb-99aa-25a30fabe8b7.png)

![image](https://user-images.githubusercontent.com/28957748/121935562-c3361780-cd72-11eb-94ff-b0f404d14ef7.png)

## :star: Big lessons

![image](https://user-images.githubusercontent.com/28957748/121934558-a0efca00-cd71-11eb-92ca-dbdbe3cfc171.png)

## :star: Problem solved

![image](https://user-images.githubusercontent.com/28957748/121937587-085b4900-cd75-11eb-85cd-4dc2c3e10191.png)

## :star: Visualize migrations

![image](https://user-images.githubusercontent.com/28957748/121937330-c6ca9e00-cd74-11eb-9cf8-f6e23d11e517.png)

## :star: Data migration

- 2 options

![image](https://user-images.githubusercontent.com/28957748/121948040-f41d4900-cd80-11eb-9a82-76f115065046.png)

- Proper solution

![image](https://user-images.githubusercontent.com/28957748/121948399-668e2900-cd81-11eb-8d31-8185d21786bb.png)

#### :one: Add column loc
```console
bar@foo:~$ npm run migrate create add loc to posts table
```

`1622001086912_add-loc-to-posts-table.js`
```javascript
exports.shorthands = undefined;

exports.up = (pgm) => {
  pgm.sql(`
        ALTER TABLE posts
        ADD COLUMN loc POINT
    `);
};

exports.down = (pgm) => {
  pgm.sql(`
        ALTER TABLE posts
        DROP COLUMN loc
    `);
};
```

```console
bar@foo:~$ set DATABASE_URL=postgres://postgres:123456@localhost:5432/socialnetwork&&npm run migrate up
```

#### :two: Deploy new version of API that will write values to both lat/lng and loc
```javascript
await pool.query("INSERT INTO posts (lat, lng, loc) VALUES ($1, $2, $3)", [
  lat,
  lng,
  `(${lng}, ${lat})`,
]);
```

#### :three: Copy lat/lng to loc
`migrations/data/01-lng-lat-to-loc.js`
```javascript
const pg = require("pg");

const pool = new pg.Pool({
  host: "localhost",
  port: 5432,
  database: "socialnetwork",
  user: "postgres",
  password: "123456",
});

pool
  .query(
    `
    UPDATE posts
    SET loc = POINT(lng, lat)
    WHERE loc IS NULL;
`
  )
  .then(() => console.log("Data migration completed."))
  .catch((err) => console.error(err.message))
  .finally(() => pool.end());
```

```console
foo@bar:~# node migrations/data/01-lng-lat-to-loc.js
```

#### :four: Update code to only write to loc column

```javascript
await pool.query("INSERT INTO posts (loc) VALUES ($1)", [
  `(${lng}, ${lat})`,
]);
```

#### :five: Drop columns lat/lng
```console
bar@foo:~$ npm run migrate create drop lat and lng from posts
```

`1622004951161_drop-lat-and-lng-from-posts.js`
```javascript
exports.shorthands = undefined;

exports.up = (pgm) => {
  pgm.sql(`
        ALTER TABLE posts
        DROP COLUMN lat,
        DROP COLUMN lng;
    `);
};

exports.down = (pgm) => {
  pgm.sql(`
        ALTER TABLE posts
        ADD COLUMN lat NUMERIC,
        ADD COLUMN lng NUMERIC;
    `);
};
```

```console
bar@foo:~$ set DATABASE_URL=postgres://postgres:123456@localhost:5432/socialnetwork&&npm run migrate up
```
