### Table: Signups

| Column Name  | Type     |
|--------------|----------|
| user_id      | int      |
| time_stamp   | datetime |

- `user_id` is the column containing unique values for this table.
- Each row contains information about the signup time for the user with ID `user_id`.

### Table: Confirmations

| Column Name  | Type     |
|--------------|----------|
| user_id      | int      |
| time_stamp   | datetime |
| action       | ENUM     |

- `(user_id, time_stamp)` is the primary key (a combination of columns with unique values) for this table.
- `user_id` is a foreign key (reference column) to the `Signups` table.
- `action` is an ENUM type with values ('confirmed', 'timeout').
- Each row in this table indicates that the user with ID `user_id` requested a confirmation message at `time_stamp`, and the message was either confirmed ('confirmed') or expired without confirming ('timeout').

### Problem:
The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user who did not request any confirmation messages is 0. The confirmation rate should be rounded to two decimal places.

### Task:
Write a solution to find the confirmation rate of each user.

### Result:
Return the result table in any order.

#### Example 1:

**Input:**  
**Signups table:**

| user_id | time_stamp          |
|---------|---------------------|
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |

**Confirmations table:**

| user_id | time_stamp          | action    |
|---------|---------------------|-----------|
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |

**Output:**

| user_id | confirmation_rate |
|---------|-------------------|
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |

**Explanation:**
- User 6 did not request any confirmation messages. The confirmation rate is 0.
- User 3 made 2 requests, and both timed out. The confirmation rate is 0.
- User 7 made 3 requests, and all were confirmed. The confirmation rate is 1.
- User 2 made 2 requests, one confirmed and one timed out. The confirmation rate is 1 / 2 = 0.5.
**Solution**
```sql
SELECT 
  signups.user_id, 
  round(
    ifnull(
      SUM(
        CASE WHEN (
          confirmations.action = 'confirmed'
        ) THEN 1 END
      )/ COUNT(confirmations.action), 
      0
    ), 
    2
  ) AS confirmation_rate 
FROM 
  signups 
  LEFT JOIN confirmations ON signups.user_id = confirmations.user_id 
GROUP BY 
  signups.user_id 
ORDER BY 
  confirmation_rate;