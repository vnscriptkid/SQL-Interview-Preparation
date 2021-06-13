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
- Assumptions

![image](https://user-images.githubusercontent.com/28957748/121798247-6a219300-cc4f-11eb-8323-879211143aa9.png)

- Fomular

![image](https://user-images.githubusercontent.com/28957748/121798889-0dc07280-cc53-11eb-8e93-7a8f13f88920.png)

- Startup vs Total cost

![image](https://user-images.githubusercontent.com/28957748/121798964-97704000-cc53-11eb-93fb-727f1d764179.png)

## Planning before and after indexing, comparing different plans

```sql
-- Size of table 'likes'
SELECT COUNT(*) FROM likes
-- 752009

-- How many pages needed for 'likes' table
SELECT relpages FROM pg_class WHERE relname='likes';
-- 4790

-- Get likes before date '2011-01-01'
SELECT COUNT(*) FROM likes
WHERE created_at < '2013-01-01';
-- 63013 ~ 1/10 table size
```

![image](https://user-images.githubusercontent.com/28957748/121799395-136b8780-cc56-11eb-9355-ef9db7850454.png)

```sql
-- Create index on 'create_at' column
CREATE INDEX likes_created_at_idx ON likes(created_at);
-- See planning of the last query
EXPLAIN SELECT * FROM likes WHERE created_at < '2013-01-01';
```

![image](https://user-images.githubusercontent.com/28957748/121800012-a659f100-cc59-11eb-89ba-64b16df117f8.png)

```sql
-- Change filter condition 
EXPLAIN SELECT * FROM likes
WHERE created_at > '2013-01-01';
```

![image](https://user-images.githubusercontent.com/28957748/121800126-3d26ad80-cc5a-11eb-87a6-82887e02368a.png)

:question: What happened?
- Postgres actively chose the plan with lower cost
  - seq scan: 752009 * 0.01 + 4790 * 1 ~ 12310.09 :rocket:
  - with index: 688393 * 0.01 + (688393 / 752009) * 4790 * 4 ~ 24423.094930206953 🏃

## :star: Sources
- https://www.depesz.com/2013/05/09/explaining-the-unexplainable-part-3/
- https://www.depesz.com/2013/04/27/explaining-the-unexplainable-part-2/
- https://www.depesz.com/2013/04/16/explaining-the-unexplainable/
- https://www.geekytidbits.com/database-deadlocks/
- https://www.geekytidbits.com/performance-tuning-postgres/
- https://www.youtube.com/watch?v=clrtT_4WBAw
- https://gocardless.com/blog/debugging-the-postgres-query-planner/
