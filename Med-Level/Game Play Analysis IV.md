## Table: Activity

| Column Name  | Type    |
|--------------|---------|
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |

**Primary Key:** (player_id, event_date) - Combination of columns with unique values.

This table shows the activity of players of some games. Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on a specific day using a device.

---

### Problem Statement:

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

---

### Example:

**Input:**

Activity table:

| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-03-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

**Output:**

| fraction |
|----------|
| 0.33     |

**Explanation:**  
Only the player with id 1 logged back in after the first day he had logged in. So the answer is 1/3 = 0.33.

---

### Solution:

```sql
WITH FirstLogin AS (
    SELECT player_id, MIN(event_date) AS first_login_date
    FROM Activity
    GROUP BY player_id
),
ConsecutiveLogin AS (
    SELECT a.player_id
    FROM Activity a
    JOIN FirstLogin f ON a.player_id = f.player_id
    WHERE a.event_date = DATE_ADD(f.first_login_date, INTERVAL 1 DAY)
)
SELECT 
    ROUND(COUNT(DISTINCT c.player_id) / COUNT(DISTINCT a.player_id), 2) AS fraction
FROM Activity a
LEFT JOIN ConsecutiveLogin c ON a.player_id = c.player_id;
