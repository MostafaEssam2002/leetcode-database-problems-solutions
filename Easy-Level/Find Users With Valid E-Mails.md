# Problem: Find Users with Valid Emails

## Table: Users

| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| name          | varchar |
| mail          | varchar |

- `user_id` is the primary key (column with unique values) for this table.
- This table contains information about users signed up on a website. Some e-mails are invalid.

### Definition of a Valid E-mail:
1. A valid e-mail has a prefix name and a domain.
2. **Prefix Name:**
   - May contain letters (upper or lower case), digits, underscore `_`, period `.`, and/or dash `-`.
   - Must start with a letter.
3. **Domain:** `@leetcode.com`.

### Objective:
Write a solution to find users with valid e-mails and return the result table in any order.

---

## Example

### Input:
Users table:

| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 2       | Jonathan  | jonathanisgreat         |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
| 5       | Marwan    | quarz#2020@leetcode.com |
| 6       | David     | david69@gmail.com       |
| 7       | Shapiro   | .shapo@leetcode.com     |

### Output:

| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |

### Explanation:
1. User 2: Missing domain.
2. User 5: Contains invalid `#` character.
3. User 6: Domain is not `@leetcode.com`.
4. User 7: Prefix starts with a period (`.`).

---

## SQL Query

```sql
SELECT user_id, name, mail
FROM Users
WHERE 
    mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$';
```

### Explanation of the `REGEXP` Pattern:
1. `^` - Matches the beginning of the string.
2. `[a-zA-Z]` - Ensures the first character of the prefix is a letter (uppercase or lowercase).
3. `[a-zA-Z0-9_.-]*` - Allows the rest of the prefix to contain letters, digits, underscores `_`, periods `.`, or dashes `-` (0 or more occurrences).
4. `@leetcode\\.com` - Matches the domain exactly (`@leetcode.com`). The `\\` escapes the dot `.` because it is a special character in regular expressions.
5. `$` - Matches the end of the string.

---

## Expected Output
For the provided input, the query will return:

| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |

---
