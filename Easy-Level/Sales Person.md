# Find Salespersons Who Did Not Have Orders for Company "RED"

## **Table: `SalesPerson`**

| Column Name     | Type    |
|-----------------|---------|
| sales_id        | int     |
| name            | varchar |
| salary          | int     |
| commission_rate | int     |
| hire_date       | date    |

- **`sales_id`** is the **primary key**.
- This table contains details about salespeople, including their names, salaries, commission rates, and hire dates.

---

## **Table: `Company`**

| Column Name | Type    |
|-------------|---------|
| com_id      | int     |
| name        | varchar |
| city        | varchar |

- **`com_id`** is the **primary key**.
- This table contains details about companies, including their names and locations.

---

## **Table: `Orders`**

| Column Name | Type  |
|-------------|-------|
| order_id    | int   |
| order_date  | date  |
| com_id      | int   |
| sales_id    | int   |
| amount      | int   |

- **`order_id`** is the **primary key**.
- **`com_id`** is a **foreign key** referencing the `Company` table.
- **`sales_id`** is a **foreign key** referencing the `SalesPerson` table.
- This table contains information about orders, including the company and salesperson involved.

---

## **Problem Statement**

Write a SQL query to find **all salespersons who have NOT placed any orders for the company named "RED"**.

Return the result table **in any order**.

---

## **Example 1**

### **Input Data**

### **`SalesPerson` Table**
| sales_id | name | salary | commission_rate | hire_date  |
|----------|------|--------|-----------------|------------|
| 1        | John | 100000 | 6               | 4/1/2006   |
| 2        | Amy  | 12000  | 5               | 5/1/2010   |
| 3        | Mark | 65000  | 12              | 12/25/2008 |
| 4        | Pam  | 25000  | 25              | 1/1/2005   |
| 5        | Alex | 5000   | 10              | 2/3/2007   |

### **`Company` Table**
| com_id | name   | city     |
|--------|--------|----------|
| 1      | RED    | Boston   |
| 2      | ORANGE | New York |
| 3      | YELLOW | Boston   |
| 4      | GREEN  | Austin   |

### **`Orders` Table**
| order_id | order_date | com_id | sales_id | amount |
|----------|------------|--------|----------|--------|
| 1        | 1/1/2014   | 3      | 4        | 10000  |
| 2        | 2/1/2014   | 4      | 5        | 5000   |
| 3        | 3/1/2014   | 1      | 1        | 50000  |
| 4        | 4/1/2014   | 1      | 4        | 25000  |

---

### **Expected Output**
| name  |
|-------|
| Amy   |
| Mark  |
| Alex  |

---

### **Explanation**
- **Salespersons `John` and `Pam` have orders related to `RED`** (`com_id = 1`).
- **Salespersons `Amy`, `Mark`, and `Alex` have NOT placed any orders for `RED`**.
- The result includes **only those who never dealt with "RED"**.

---

## **SQL Solution**
```sql
SELECT DISTINCT salesperson.name 
FROM salesperson 
LEFT JOIN orders ON orders.sales_id = salesperson.sales_id 
LEFT JOIN company ON company.com_id = orders.com_id 
WHERE salesperson.sales_id NOT IN (
    SELECT DISTINCT sales_id 
    FROM orders 
    JOIN company ON orders.com_id = company.com_id 
    WHERE company.name = 'RED'
);
