## :star: Phases of planning and executing queries

![image](https://user-images.githubusercontent.com/28957748/121779454-e2437680-cbc5-11eb-92e1-803f491d6d49.png)

## :star: `EXPLAIN` vs `EXPLAIN ANALYZE`

```sql
EXPLAIN SELECT username, contents
FROM users JOIN comments
ON users.id = comments.user_id
WHERE username = 'Alyson14';
```

![image](https://user-images.githubusercontent.com/28957748/121781358-7f0a1200-cbce-11eb-8ffe-af8ff7cff1f9.png)

```sql
EXPLAIN ANALYZE SELECT username, contents
FROM users JOIN comments
ON users.id = comments.user_id
WHERE username = 'Alyson14';
```

![image](https://user-images.githubusercontent.com/28957748/121781311-4f5b0a00-cbce-11eb-8773-ea4bb537fdcf.png)

:question: But from where Postgres bases on, in order to predict executing time

```sql
SELECT *
FROM pg_stats
WHERE tablename = 'users';
```

## :star: Calculating query cost
- Assuming
  - Loading a random block is 4 times longer than loading a block sequencially
