# Find First Login Date for Each Player

## **Table: `Activity`**

| Column Name  | Type    |
|--------------|---------|
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |

- **(player_id, event_date)** is the **primary key** (a combination of columns with unique values).
- Each row represents **a player's login activity** on a specific date, using a certain device.
- `games_played` represents the **number of games played** before logging out.

---

## **Problem Statement**

Write a SQL query to **find the first login date for each player**.

The result table can be returned in **any order**.

---

## **Example 1**

### **Input Data**

### **`Activity` Table**
| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

---

### **Expected Output**
| player_id | first_login |
|-----------|------------|
| 1         | 2016-03-01 |
| 2         | 2017-06-25 |
| 3         | 2016-03-02 |

---

### **Explanation:**
- **Player `1`** logged in on **2016-03-01** and **2016-05-02** → **First login: `2016-03-01`**
- **Player `2`** logged in only on **2017-06-25** → **First login: `2017-06-25`**
- **Player `3`** logged in on **2016-03-02** and **2018-07-03** → **First login: `2016-03-02`**

---

## **SQL Solution**
```sql
SELECT player_id, MIN(event_date) AS first_login  
FROM Activity  
GROUP BY player_id  
ORDER BY player_id;
