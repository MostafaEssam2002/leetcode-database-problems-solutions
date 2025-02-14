# Department Highest Salary

## Problem Description

### Table: Employee

| Column Name  | Type    |
|--------------|---------|
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |

- `id` is the **primary key** (column with unique values) for this table.
- `departmentId` is a **foreign key** referencing the `id` column in the **Department** table.
- Each row contains the **ID, name, and salary** of an employee, along with the **ID of their department**.

### Table: Department

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |

- `id` is the **primary key** (column with unique values) for this table.
- It is **guaranteed** that the department name is **not NULL**.
- Each row represents the **ID of a department** and its **name**.

### Task:
Write a solution to **find employees who have the highest salary in each department**.

Return the result table **in any order**.

### Example:

#### Example 1:

##### Input:
**Employee table:**

| id | name  | salary | departmentId |
|----|-------|--------|--------------|
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |

**Department table:**

| id | name  |
|----|-------|
| 1  | IT    |
| 2  | Sales |

##### Output:

| Department | Employee | Salary |
|------------|----------|--------|
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
| IT         | Max      | 90000  |

#### Explanation:
- **Jim and Max** both have the **highest salary (90000)** in the **IT department**.
- **Henry** has the **highest salary (80000)** in the **Sales department**.

---

## Solution:

```sql
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM Employee e
JOIN Department d ON e.departmentId = d.id
WHERE e.salary = (
    SELECT MAX(salary)
    FROM Employee
    WHERE departmentId = e.departmentId
);
