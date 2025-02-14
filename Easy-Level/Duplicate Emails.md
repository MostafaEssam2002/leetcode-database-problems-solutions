# Duplicate Emails

## Problem Description

### Table: Person

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| email       | varchar |

- `id` is the **primary key** (column with unique values) for this table.
- Each row of this table contains an email. The emails will **not** contain uppercase letters.

### Task:
Write a query to **report all duplicate emails** in the table.  
The result table can be returned **in any order**.

### Example:

#### Input:
**Person table:**

| id | email   |
|----|---------|
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |

#### Output:

| Email   |
|---------|
| a@b.com |

#### Explanation:
- `a@b.com` appears **twice**, so it is included in the output.
- `c@d.com` appears only **once**, so it is **not** included.

---

## Solution:

```sql
SELECT email 
FROM Person 
GROUP BY email 
HAVING COUNT(email) > 1;
