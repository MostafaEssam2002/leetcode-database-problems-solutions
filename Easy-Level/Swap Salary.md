# Swap 'm' and 'f' in Salary Table

## **Table: `Salary`**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| sex         | ENUM    |
| salary      | int     |

- **`id`** is the **primary key**.
- **`sex`** is an `ENUM` with values **('m', 'f')**.
- The table contains employee details, including their gender and salary.

---

## **Problem Statement**
Write a SQL query to **swap all 'f' and 'm' values** in the `sex` column:
- Change all **'m'** to **'f'**.
- Change all **'f'** to **'m'**.

- You must **write a single `UPDATE` statement**.
- Do **not use any intermediate tables** or `SELECT` statements.

---

## **Example 1**

### **Input Data**

### **`Salary` Table**
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

---

### **Expected Output**
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

---

### **Explanation**
- **Row (1, A) and (3, C) were changed from 'm' → 'f'**.
- **Row (2, B) and (4, D) were changed from 'f' → 'm'**.

---

## **SQL Solution**
```sql
UPDATE Salary 
SET sex = CASE 
             WHEN sex = 'm' THEN 'f' 
             ELSE 'm' 
          END;
