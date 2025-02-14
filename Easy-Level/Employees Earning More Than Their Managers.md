# Employees Earning More Than Their Managers

## Problem Description

### Table: Employee

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |

- `id` is the **primary key** (column with unique values) for this table.
- Each row of this table indicates the **ID** of an employee, their **name**, **salary**, and the **ID of their manager**.

### Task:
Write a query to find the **employees who earn more than their managers**.  
The result table can be returned **in any order**.

### Example:

#### Input:
**Employee table:**

| id | name  | salary | managerId |
|----|-------|--------|-----------|
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | Null      |
| 4  | Max   | 90000  | Null      |

#### Output:

| Employee |
|----------|
| Joe      |

#### Explanation:
- **Joe** earns **$70,000**, while his manager (**Sam**) earns **$60,000**, so **Joe** is included in the result.
- **Henry** earns **$80,000**, but his manager (**Max**) earns **$90,000**, so Henry is **not** included.
- **Sam** and **Max** do **not** have managers, so they are **not** included.

---

## Solution:

```sql
SELECT s1.name AS Employee 
FROM Employee s1 
LEFT JOIN Employee s2 
ON s1.managerId = s2.id 
WHERE s1.salary > s2.salary;
