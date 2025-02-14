# Problem Description

You are given a table `MyNumbers` which contains an integer column `num`. This table may contain duplicates (there is no primary key constraint on the table).

Your task is to find the largest single number. A **single number** is a number that appeared only once in the `MyNumbers` table.

If there are no single numbers, you should return `null`.

---

## Example Input

### `MyNumbers` Table:
| num |
|-----|
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 5   |
| 6   |

---

## Example Output

| num |
|-----|
| 6   |

### Explanation:
The single numbers are `1`, `4`, `5`, and `6`. Since `6` is the largest single number, we return it.

---

### Example Input 2:

### `MyNumbers` Table:
| num |
|-----|
| 8   |
| 8   |
| 7   |
| 7   |
| 3   |
| 3   |
| 3   |

---

### Example Output 2:

| num  |
|------|
| null |

### Explanation:
There are no single numbers in the input table, so we return `null`.

---

## Solution

### SQL Query

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num, COUNT(num) AS mxnum
    FROM MyNumbers
    GROUP BY num
) AS tst
WHERE mxnum = 1;
