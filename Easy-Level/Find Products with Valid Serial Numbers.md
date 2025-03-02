# Find Products with Valid Serial Numbers

## Table: products

| Column Name  | Type    |
|-------------|---------|
| product_id  | int     |
| product_name | varchar |
| description | varchar |

- `product_id` is the unique key for this table.
- Each row in the table represents a product with its unique ID, name, and description.

## Problem Statement

Write a SQL query to find all products whose `description` contains a **valid serial number pattern**. A valid serial number follows these rules:

1. It **starts with** the letters **"SN"** (case-sensitive).
2. It is **followed by exactly 4 digits**.
3. It **must contain a hyphen (-) followed by exactly 4 more digits**.
4. The serial number **must be within the description** (it may not necessarily start at the beginning).
5. Return the result table **ordered by `product_id` in ascending order**.

---

## Example

### **Input:**  
#### `products` table:

| product_id | product_name | description                                        |
|------------|-------------|------------------------------------------------------|
| 1          | Widget A     | This is a sample product with SN1234-5678          |
| 2          | Widget B     | A product with serial SN9876-1234 in the description |
| 3          | Widget C     | Product SN1234-56789 is available now              |
| 4          | Widget D     | No serial number here                              |
| 5          | Widget E     | Check out SN4321-8765 in this description          |

---

### **Output:**

| product_id | product_name | description                                        |
|------------|-------------|------------------------------------------------------|
| 1          | Widget A     | This is a sample product with SN1234-5678          |
| 2          | Widget B     | A product with serial SN9876-1234 in the description |
| 5          | Widget E     | Check out SN4321-8765 in this description          |

---

### **Explanation:**
- **Product 1**: ✅ Valid serial number **SN1234-5678**.
- **Product 2**: ✅ Valid serial number **SN9876-1234**.
- **Product 3**: ❌ **Invalid** serial number **SN1234-56789** (**5 digits after hyphen**).
- **Product 4**: ❌ **No serial number**.
- **Product 5**: ✅ Valid serial number **SN4321-8765**.

---

## **SQL Solution**
```sql
SELECT * 
FROM products
WHERE description REGEXP '\\bSN[0-9]{4}-[0-9]{4}\\b'
ORDER BY product_id;
