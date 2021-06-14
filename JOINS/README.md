## 1. 4 kinds of JOINS
- INNER JOIN
- RIGHT JOIN
- LEFT JOIN
- FULL JOIN

![image](https://i.pinimg.com/originals/bc/0c/8b/bc0c8ba4d12051502a68bade9bba4bc5.png)

## Order matters ?
- Yes, in case of: `RIGHT JOIN` && `LEFT JOIN`
- No, in case of `INNER JOIN` && `FULL JOIN`

## 3-way JOIN
- :rocket: Who is commenting own their own photos?
![image](https://user-images.githubusercontent.com/28957748/120635261-76129580-c496-11eb-84dc-9fda71bca97a.png)

## Visualize `LEFT JOIN`

![image](https://user-images.githubusercontent.com/28957748/121924018-b6f78d80-cd65-11eb-88ee-e95308ed3105.png)

```sql
SELECT *
FROM likes
LEFT JOIN posts ON likes.post_id = posts.id
LEFT JOIN comments ON likes.comment_id = comments.id;
```
