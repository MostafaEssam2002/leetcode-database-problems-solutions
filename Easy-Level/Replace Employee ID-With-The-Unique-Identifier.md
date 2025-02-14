# SQL Solution for Finding Unique IDs of Employees

## Problem Description
We have two tables:
1. **Employees** table:
   - Contains the `id` and `name` of employees.
   - `id` is the primary key.

2. **EmployeeUNI** table:
   - Contains the `id` and the corresponding `unique_id` for employees.
   - `(id, unique_id)` is the composite primary key.

The task is to display the `unique_id` and `name` for each employee. If an employee does not have a corresponding `unique_id`, show `NULL` for the `unique_id`.

### Input Example

#### Employees Table:
| id  | name      |
| ----|-----------|
| 1   | Alice     |
| 7   | Bob       |
| 11  | Meir      |
| 90  | Winston   |
| 3   | Jonathan  |

#### EmployeeUNI Table:
| id  | unique_id |
| ----|-----------|
| 3   | 1         |
| 11  | 2         |
| 90  | 3         |

### Expected Output:
| unique_id | name      |
|-----------|-----------|
| NULL      | Alice     |
| NULL      | Bob       |
| 2         | Meir      |
| 3         | Winston   |
| 1         | Jonathan  |

### Explanation:
- Alice and Bob do not have entries in the `EmployeeUNI` table, so their `unique_id` is `NULL`.
- The `unique_id` of Meir is `2`, Winston is `3`, and Jonathan is `1` based on the mapping in the `EmployeeUNI` table.

---

## SQL Solution
```sql
SELECT 
    EmployeeUNI.unique_id, 
    Employees.name 
FROM 
    Employees 
LEFT JOIN 
    EmployeeUNI 
ON 
    EmployeeUNI.id = Employees.id;
