# SQL Query to Join `Person` and `Address` Tables

## **Table: `Person`**

| Column Name | Type    |
|-------------|---------|
| personId    | int     |
| lastName    | varchar |
| firstName   | varchar |

- `personId` is the **primary key** (column with unique values) for this table.
- This table contains information about the **ID of persons** along with their first and last names.

---

## **Table: `Address`**

| Column Name | Type    |
|-------------|---------|
| addressId   | int     |
| personId    | int     |
| city        | varchar |
| state       | varchar |

- `addressId` is the **primary key** (column with unique values) for this table.
- Each row contains information about the **city and state** of a person whose `personId` exists in the `Person` table.

---

## **Problem Statement**

Write a SQL query to **report the first name, last name, city, and state** of each person in the `Person` table.  
If the address of a `personId` is **not present** in the `Address` table, **return `NULL` instead**.

The result table can be returned in **any order**.

---

## **Example 1**

### **Input Data**

### **`Person` Table**
| personId | lastName | firstName |
|----------|----------|-----------|
| 1        | Wang     | Allen     |
| 2        | Alice    | Bob       |

### **`Address` Table**
| addressId | personId | city          | state      |
|-----------|----------|---------------|------------|
| 1         | 2        | New York City | New York   |
| 2         | 3        | Leetcode      | California |

---

### **Expected Output**
| firstName | lastName | city          | state    |
|-----------|----------|---------------|----------|
| Allen     | Wang     | NULL          | NULL     |
| Bob       | Alice    | New York City | New York |

---

### **Explanation:**
- `personId = 1` does **not** exist in the `Address` table, so their **city and state are `NULL`**.
- `personId = 2` has an address in the `Address` table, so their city and state are retrieved.

---

## **SQL Solution**
```sql
SELECT firstName, lastName, city, state 
FROM Person p 
LEFT JOIN Address a 
ON p.personId = a.personId;
