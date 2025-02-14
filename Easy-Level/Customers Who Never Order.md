# Find Customers Who Never Ordered Anything

## **Table: `Customers`**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |

- `id` is the **primary key** (column with unique values) for this table.
- Each row represents the **ID and name** of a customer.

---

## **Table: `Orders`**

| Column Name | Type |
|-------------|------|
| id          | int  |
| customerId  | int  |

- `id` is the **primary key** (column with unique values) for this table.
- `customerId` is a **foreign key** referencing `id` from the `Customers` table.
- Each row represents an **order** and the `customerId` of the customer who placed it.

---

## **Problem Statement**

Write a SQL query to **find all customers who never ordered anything**.

The result table can be returned in **any order**.

---

## **Example 1**

### **Input Data**

### **`Customers` Table**
| id | name  |
|----|-------|
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |

### **`Orders` Table**
| id | customerId |
|----|------------|
| 1  | 3          |
| 2  | 1          |

---

### **Expected Output**
| Customers |
|-----------|
| Henry     |
| Max       |

---

### **Explanation:**
- **Customer `Henry` (`id = 2`)** has **no matching order** in the `Orders` table.
- **Customer `Max` (`id = 4`)** has **no matching order** in the `Orders` table.
- **Customer `Joe` (`id = 1`)** and **Customer `Sam` (`id = 3`)** have placed orders, so they are **not** included in the result.

---

## **SQL Solution**
```sql
SELECT name AS Customers 
FROM Customers 
LEFT JOIN Orders 
ON Orders.customerId = Customers.id 
WHERE Orders.id IS NULL;
