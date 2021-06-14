## Dependencies
- `package.json`
```json
  "node-pg-migrate": "^5.9.0",
  "pg": "^8.6.0"
```

## Workflow
- `package.json`
```json
  "scripts": {
    "migrate": "node-pg-migrate"
  }
```

- Create a new migration file
```console
foo@bar:~$ npm run migrate create table comments
```

- Write actual changes to newly created migration file `migrations/1621937157797_table-comments.js`

```javascript

exports.shorthands = undefined;

exports.up = (pgm) => {
  pgm.sql(`
        CREATE TABLE comments (
            id SERIAL PRIMARY KEY,
            created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
            updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
            contents VARCHAR(240) NOT NULL
        );
    `);
};

exports.down = (pgm) => {
  pgm.sql(`
        DROP TABLE comments;
    `);
};
```

- Apply migration (for windows cmd)
```console
foo@bar:~$ set DATABASE_URL=postgres://postgres:123456@localhost:5432/socialnetwork&&npm run migrate up
```

- Revert to previous state of database
```console
foo@bar:~$ set DATABASE_URL=postgres://postgres:123456@localhost:5432/socialnetwork&&npm run migrate down
```

