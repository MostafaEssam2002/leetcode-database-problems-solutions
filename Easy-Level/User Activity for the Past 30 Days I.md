# Problem Description

You are given a table `Activity` that records user activities on a social media website.

### Table: Activity

| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |

- This table may have duplicate rows.
- The `activity_type` column is an ENUM with possible values: `'open_session'`, `'end_session'`, `'scroll_down'`, `'send_message'`.
- Each session belongs to exactly one user.

---

## Task

Write a solution to find the **daily active user count** for a period of 30 days ending on `2019-07-27` inclusively. A user is considered active on a given day if they performed at least one activity on that day.

The result table should contain:
- `day`: The activity date.
- `active_users`: The count of distinct users who were active on that day.

The result can be returned in any order.

---

## Example Input

### `Activity` Table:
| user_id | session_id | activity_date | activity_type  |
|---------|------------|---------------|----------------|
| 1       | 1          | 2019-07-20    | open_session   |
| 1       | 1          | 2019-07-20    | scroll_down    |
| 1       | 1          | 2019-07-20    | end_session    |
| 2       | 4          | 2019-07-20    | open_session   |
| 2       | 4          | 2019-07-21    | send_message   |
| 2       | 4          | 2019-07-21    | end_session    |
| 3       | 2          | 2019-07-21    | open_session   |
| 3       | 2          | 2019-07-21    | send_message   |
| 3       | 2          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session   |
| 4       | 3          | 2019-06-25    | end_session    |

---

## Example Output

| day        | active_users |
|------------|--------------|
| 2019-07-20 | 2            |
| 2019-07-21 | 2            |

### Explanation:
1. On `2019-07-20`, users `1` and `2` were active.
2. On `2019-07-21`, users `2` and `3` were active.
3. Days with zero active users are not included in the result.

---

## Solution

### SQL Query

```sql
SELECT
    activity_date AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM
    Activity
WHERE
    activity_date BETWEEN '2019-06-28' AND '2019-07-27'
GROUP BY
    activity_date;
