# Problem Description

### Table: Employees

| Column Name | Type    |
|-------------|---------|
| employee_id | int     |
| name        | varchar |
| manager_id  | int     |
| salary      | int     |

- `employee_id` is the primary key for this table.
- This table contains information about employees, their salary, and the ID of their manager. Some employees do not have a manager (`manager_id` is `null`).

### Problem Statement

Find the IDs of employees whose salary is strictly less than $30,000 and whose manager has left the company. When a manager leaves the company, their information is deleted from the `Employees` table, but the employee still has the manager's `manager_id`.

**Output Requirements:**
- Return the `employee_id` of such employees ordered by `employee_id`.

---

### Example Input

**Employees Table:**

| employee_id | name      | manager_id | salary |
|-------------|-----------|------------|--------|
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | null       | 50937  |
| 11          | Joziah    | 6          | 28485  |

---

### Example Output

| employee_id |
|-------------|
| 11          |

**Explanation:**
- Employees with a salary less than $30,000: Kalel (1) and Joziah (11).
- Kalel’s manager is Joziah (11), who is still in the company.
- Joziah’s manager is employee 6, who has left the company (no record of employee 6).

---

### SQL Solution

```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
  AND manager_id IS NOT NULL
  AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
