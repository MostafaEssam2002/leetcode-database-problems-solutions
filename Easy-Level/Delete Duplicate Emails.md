### Problem Description:

#### Table: Person

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| email       | varchar |

- `id` is the primary key (column with unique values) for this table.
- Each row of this table contains an email. The emails will not contain uppercase letters.

You need to write a solution to delete all duplicate emails, keeping only one unique email with the smallest `id`.

For SQL users, please note that you are supposed to write a DELETE statement and not a SELECT one.

After running your script, the answer shown is the Person table. The driver will first compile and run your piece of code and then show the Person table. The final order of the Person table does not matter.

---

### Example:

**Input:**
Person table:

| id  | email            |
|-----|------------------|
| 1   | john@example.com |
| 2   | bob@example.com  |
| 3   | john@example.com |

**Output:**
| id  | email            |
|-----|------------------|
| 1   | john@example.com |
| 2   | bob@example.com  |

**Explanation:**  
`john@example.com` is repeated two times. We keep the row with the smallest `id` (which is `id = 1`).

---

### SQL Solution:

```sql
-- Delete duplicate emails, keeping only the one with the smallest id
DELETE Person
FROM Person
JOIN Person p1 ON p1.email = Person.email
WHERE p1.id < Person.id;
