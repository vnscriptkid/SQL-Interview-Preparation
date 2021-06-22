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

- What?
	- Produce a virtual table on the fly (in the RAM !?!) at the time referencing table
	- vs CTE ? 
		- View can be defined once and refer anywhere
		- CTE is like naming a piece of query and refer it in the same context

- Example
```sql
-- define a new view
CREATE VIEW tags AS (
	SELECT id, created_at, post_id, user_id, 'caption' AS type FROM caption_tags
	UNION ALL
	SELECT id, created_at, post_id, user_id, 'photo' AS type FROM photo_tags
);

-- some other context: find top 10 tagged users
SELECT username, COUNT(*) FROM tags
JOIN users ON users.id = tags.user_id
GROUP BY username
ORDER BY count DESC
LIMIT 10;

-- delete the view
DROP VIEW tags;

-- update existing view
CREATE OR REPLACE VIEW recent_posts AS (
	SELECT * FROM posts
	ORDER BY created_at DESC
	LIMIT 10
)

-- change definition later
CREATE OR REPLACE VIEW recent_posts AS (
	SELECT * FROM posts
	ORDER BY created_at DESC
	LIMIT 15
)
```

## :star: Materialized views
- What ?
	-  Materialized view = view + cache
	-  Cache data from the last refresh (or when it was first ran) and return it when view is referred next time
- When ?
	- Long running query
	- Data from query does not change often

```sql
-- create new materialized view
CREATE MATERIALIZED VIEW weekly_likes AS (
	SELECT
		date_trunc('week', COALESCE(posts.created_at, comments.created_at)) AS week,
		COUNT(posts.id) AS num_likes_for_posts,
		COUNT(comments.id) AS num_likes_for_comments
	FROM likes
	LEFT JOIN posts ON likes.post_id = posts.id
	LEFT JOIN comments ON likes.comment_id = comments.id
	GROUP BY week
	ORDER BY week
) WITH DATA;

-- get brand new data and cache it
REFRESH MATERIALIZED VIEW weekly_likes;

-- take data from the cache
SELECT * FROM weekly_likes;
```

## :star: LEAD() function
- What ❓ Provide access to the next row
- When ❓ Compare value of current row with next row
- How 🔯 
	- Example: Compare the sales of the current year with sales of the next year for each product group
	- ![image](https://user-images.githubusercontent.com/28957748/122888781-5cb18a80-d36c-11eb-9ebe-b1b07a818ba4.png)
	```sql
	SELECT
		year, 
		amount,
		group_id,
		LEAD(amount,1) OVER (
			PARTITION BY group_id
			ORDER BY year
		) next_year_sales
	FROM
		sales;
	```
	- Explainations:
		- The `PARTITION BY` clause distributes rows into product groups (or partitions) specified by group id
		- The `ORDER BY` clause sorts rows in each product group by years in ascending order.
		- The `LEAD()` function returns the sales of the next year from the current year for each product group.
