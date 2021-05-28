## 1. Introduction to DB Design
- High level: It's about deciding what information to store, how to model it and what are the relationships between data
- Low level: Create tables storing data, define schema for each table, how tables relate to each other.
- Where to start?
    - UI design
    - Features

## 2. Keys
- Primary :key: 
    - Specify one unique record in table
    - 2 ways: auto increment (`SERIAL`) OR UUID (Universally Unique Identifier)
    - auto-indexed by DB (along with UNIQUE cols)
- Foreign :old_key:
    - Used to relate 2 tables
    - Match up with __Primary__ :key: from another table
    - How to make one:
      ```sql
      boat_id INTEGER REFERENCES boats(id)
      ```
    - Why we need it (instead of just using raw INTEGER field `boat_id`)?
        - Raw fields work just fine in case of retrieving data
        - Foreign :old_key: provides data __consistency__ through constraints around deletion (RAN CD 😆)
            - ON DELETE RESTRICTION
            - ON DELETE NO ACTION
            - ON DELETE CASCADE
            - ON DELETE SET NULL
            - ON DELETE SET DEFAULT
## 3. Four kinds of relationships
- One-To-One
- One-To-Many
    - Foreign :old_key: is on Many's side
    - Example: 1 Post __has many__ Comments
- Many-To-One: 
    - Identical to __One-To-Many__, just different depending on which perspective are you looking from
    - Example: From Comments perspective, it's __Many-To-One__ relationship
- Many-To-Many
