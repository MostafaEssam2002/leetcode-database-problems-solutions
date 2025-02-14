## Latest Logins in 2020

### Table: Logins

```plaintext

| Column Name    | Type     |
|----------------|----------|
| user_id        | int      |
| time_stamp     | datetime |

```

- `(user_id, time_stamp)` is the primary key (combination of columns with unique values) for this table.
- Each row contains information about the login time for the user with ID `user_id`.

### Problem Statement
Write a solution to report the latest login for all users in the year **2020**. Do not include the users who did not log in during 2020.

The result table can be returned in any order.

### Example
#### Input:
```plaintext
Logins table:

| user_id | time_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 6       | 2021-04-21 14:06:06 |
| 6       | 2019-03-07 00:18:15 |
| 8       | 2020-02-01 05:10:53 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
| 2       | 2019-08-25 07:59:08 |
| 14      | 2019-07-14 09:00:00 |
| 14      | 2021-01-06 11:59:59 |
```

#### Output:
```plaintext
| user_id | last_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
```

### Explanation
- **User 6** logged in **three times**, but only **once in 2020**, so we include this login in the result.
- **User 8** logged in **twice in 2020** (February and December). We include only the **latest one (December)**.
- **User 2** logged in **once in 2020**, so we include this login in the result.
- **User 14** did **not** log in **during 2020**, so we do **not** include them.

### SQL Solution
```sql
SELECT user_id, MAX(time_stamp) AS last_stamp 
FROM logins 
WHERE YEAR(time_stamp) = '2020' 
GROUP BY user_id;
```

---
