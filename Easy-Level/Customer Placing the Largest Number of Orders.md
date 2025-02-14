# Find the Customer with the Most Orders

## **Table: `Orders`**

| Column Name     | Type  |
|-----------------|-------|
| order_number    | int   |
| customer_number | int   |

- **`order_number`** is the **primary key** (each order has a unique ID).
- This table contains information about **orders placed by customers**.

---

## **Problem Statement**

Write a SQL query to **find the `customer_number`** for the customer who has placed the **largest number of orders**.

The test cases are generated so that **exactly one customer** has more orders than any other.

---

## **Example 1**

### **Input Data**

### **`Orders` Table**
| order_number | customer_number |
|-------------|----------------|
| 1           | 1              |
| 2           | 2              |
| 3           | 3              |
| 4           | 3              |

---

### **Expected Output**
| customer_number |
|----------------|
| 3              |

---

### **Explanation**
- **Customer `1`** placed **1 order**.
- **Customer `2`** placed **1 order**.
- **Customer `3`** placed **2 orders** (**highest count**).
- The result is **customer `3`** because they have placed the most orders.

---

## **SQL Solution**
```sql
SELECT customer_number  
FROM Orders  
GROUP BY customer_number  
ORDER BY COUNT(customer_number) DESC  
LIMIT 1;
