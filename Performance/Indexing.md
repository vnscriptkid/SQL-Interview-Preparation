## :star: Full table scan

![image](https://user-images.githubusercontent.com/28957748/121631860-175aa680-caaa-11eb-836d-1ef5c837a8e8.png)


## :star: Indexing
- What is it?

![image](https://user-images.githubusercontent.com/28957748/121632497-47567980-caab-11eb-95ac-83f820242e7f.png)

- How it works? 

![image](https://user-images.githubusercontent.com/28957748/121632652-8dabd880-caab-11eb-887b-17609eedf69d.png)

- Create an index
```sql
-- Create an index with implied name
CREATE INDEX ON users(username);

-- Create an index explicitly
CREATE INDEX users_username_idx ON users(username);

-- Drop an index
DROP INDEX users_username_idx;
```

- Downsides of index:

![image](https://user-images.githubusercontent.com/28957748/121637038-04000900-cab3-11eb-91db-fa39e9924966.png)

- Auto-generated indexes
```sql
SELECT relname, relkind
FROM pg_class
WHERE relkind = 'i';
```
![image](https://user-images.githubusercontent.com/28957748/121638800-c6e94600-cab5-11eb-9ae9-e00822be7217.png)


## :star: Benchmarking queries

```sql
EXPLAIN ANALYZE SELECT *
FROM users
WHERE username = 'Emil30'
```

- Compare performance between with and without indexing

![image](https://user-images.githubusercontent.com/28957748/121634006-0a3fb680-caae-11eb-985b-8700ab1e6742.png)
![image](https://user-images.githubusercontent.com/28957748/121634065-280d1b80-caae-11eb-8e6a-32045a64283c.png)

- Compare original table vs indexing data:
```sql
SELECT pg_size_pretty(pg_relation_size('users'));
SELECT pg_size_pretty(pg_relation_size('users_username_idx'));
```
![image](https://user-images.githubusercontent.com/28957748/121636917-d0bd7a00-cab2-11eb-99b9-745623d51547.png)


## Indexing Internals

```sql
CREATE EXTENSION pageinspect;
SELECT * FROM bt_metap('users_username_idx')
SELECT * FROM bt_page_items('users_username_idx', 3)
```

![image](https://user-images.githubusercontent.com/28957748/121642215-68729680-caba-11eb-9f02-a3a4c96746b8.png)
