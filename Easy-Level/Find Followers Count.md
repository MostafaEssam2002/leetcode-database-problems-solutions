# Problem Description

You are given a table `Followers` that contains information about user relationships on a social media platform.

### Table: Followers

| Column Name | Type |
|-------------|------|
| user_id     | int  |
| follower_id | int  |

- `(user_id, follower_id)` is the primary key, meaning each user-follower pair is unique.
- Each row represents a `follower_id` who follows a `user_id`.

---

## Task

Write a query to return, for each user, the number of followers they have.

The result should:
1. Include two columns: `user_id` and `followers_count`.
2. Be ordered by `user_id` in ascending order.

---

## Example Input

### `Followers` Table:
| user_id | follower_id |
|---------|-------------|
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |

---

## Example Output

| user_id | followers_count |
|---------|-----------------|
| 0       | 1               |
| 1       | 1               |
| 2       | 2               |

### Explanation:
- The followers of user `0` are `{1}`, so the count is `1`.
- The followers of user `1` are `{0}`, so the count is `1`.
- The followers of user `2` are `{0, 1}`, so the count is `2`.

---

## Solution

### SQL Query
```sql
SELECT 
    user_id, 
    COUNT(follower_id) AS followers_count
FROM 
    Followers
GROUP BY 
    user_id
ORDER BY 
    user_id;
