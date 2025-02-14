# Fixing User Names in the `Users` Table

The problem requires updating the `name` column in the `Users` table so that only the first character is uppercase and the rest are lowercase. The result should be ordered by `user_id`.

## Table Schema
### Input Table: `Users`
| Column Name | Type    |
|-------------|---------|
| user_id     | int     |
| name        | varchar |

- `user_id` is the primary key.
- The `name` column contains strings where only the first letter needs to be capitalized.

### Example Input
#### Users Table:
| user_id | name  |
|---------|-------|
| 1       | aLice |
| 2       | bOB   |

### Expected Output
| user_id | name  |
|---------|-------|
| 1       | Alice |
| 2       | Bob   |

## SQL Query Solution
The following SQL query achieves the desired result by:
1. Extracting the first character of the `name` column and converting it to uppercase using `UPPER`.
2. Extracting the rest of the `name` and converting it to lowercase using `LOWER`.
3. Concatenating the two parts to form the correctly formatted name.

### SQL Query
```sql
SELECT 
    user_id, 
    CONCAT(UPPER(SUBSTRING(name, 1, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM 
    Users
ORDER BY 
    user_id;
