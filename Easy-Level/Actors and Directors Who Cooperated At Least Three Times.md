# Find Actor-Director Pairs with at Least Three Collaborations

## **Table: `ActorDirector`**

| Column Name  | Type |
|-------------|------|
| actor_id    | int  |
| director_id | int  |
| timestamp   | int  |

- **`timestamp`** is the **primary key**.
- The table stores records of collaborations between actors and directors.

---

## **Problem Statement**
Write a SQL query to **find all actor-director pairs** (`actor_id`, `director_id`) where:
- The **actor has collaborated with the director at least three times**.

- **Return the result in any order**.

---

## **Example 1**

### **Input Data**

### **`ActorDirector` Table**
| actor_id | director_id | timestamp |
|----------|------------|-----------|
| 1        | 1          | 0         |
| 1        | 1          | 1         |
| 1        | 1          | 2         |
| 1        | 2          | 3         |
| 1        | 2          | 4         |
| 2        | 1          | 5         |
| 2        | 1          | 6         |

---

### **Expected Output**
| actor_id | director_id |
|----------|------------|
| 1        | 1          |

---

### **Explanation**
- **Pair (1, 1)** has **3 collaborations**, so it appears in the output.
- **Pair (1, 2)** has **only 2 collaborations**, so it's **not included**.
- **Pair (2, 1)** has **only 2 collaborations**, so it's **not included**.

---

## **SQL Solution**
```sql
SELECT actor_id, director_id
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(*) >= 3;
