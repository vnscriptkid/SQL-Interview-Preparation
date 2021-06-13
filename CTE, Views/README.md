## :star: Simple CTE (Common Table Expression)
- What ?
  - Produce a table that can be refered later (in the same context)
  - A utility that breaks 1 big query into smaller parts, making it easy to read

- :question: Show username of all users who was tagged before 2010-01-07 (either in caption or photo)

![image](https://user-images.githubusercontent.com/28957748/121802148-56812700-cc65-11eb-8b74-81eff02dcd35.png)

```sql
WITH tags AS (
	SELECT user_id FROM caption_tags
	UNION
	SELECT user_id FROM photo_tags
)
SELECT username, tags.created_at 
FROM users
JOIN tags ON users.id = tags.user_id
WHERE tags.created_at < '2010-01-07';
```

## :star: Recursive CTE
- Notes
  - When? Tree or graph structure
  - How? Must use `UNION`

- Simple 101 example: Countdown from 3 to 1

```sql
WITH RECURSIVE countdown(val) AS (
	SELECT 3 AS val
	UNION
	SELECT val - 1 FROM countdown WHERE val > 1
)
SELECT * FROM countdown;
```

- Real-world example: Who should i follow ~ who do my idols follow?

![image](https://user-images.githubusercontent.com/28957748/121803526-4e78b580-cc6c-11eb-9116-413276b459ff.png)

```sql
WITH RECURSIVE suggestions(leader_id, follower_id, depth) AS (
	SELECT leader_id, follower_id, 1 AS depth 
	FROM followers 
	WHERE follower_id = 1000
		UNION
	SELECT followers.leader_id, followers.follower_id, depth + 1 
	FROM followers 
	JOIN suggestions ON suggestions.leader_id = followers.follower_id
	WHERE depth < 3
)

SELECT DISTINCT users.id, users.username
FROM suggestions
JOIN users ON users.id = suggestions.leader_id 
WHERE depth > 1
LIMIT 30;
```

## :star: Views

## :star: Materialized views
