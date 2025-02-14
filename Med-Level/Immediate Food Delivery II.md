# Problem Description

## Table: Delivery

| Column Name                 | Type    |
|-----------------------------|---------|
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |

- `delivery_id` is the column of unique values of this table.
- The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).

### Definitions:
1. **Immediate Order**: If the customer's preferred delivery date is the same as the order date, then the order is called immediate.
2. **Scheduled Order**: Otherwise, it is called scheduled.
3. **First Order**: The order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

### Task:
Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to **2 decimal places**.

### Result Format:
The result format should follow the example below.

---

## Example 1:

### Input:
**Delivery Table:**
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|-------------|-------------|------------|-----------------------------|
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-12                  |
| 4           | 3           | 2019-08-24 | 2019-08-24                  |
| 5           | 3           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
| 7           | 4           | 2019-08-09 | 2019-08-09                  |

### Output:
| immediate_percentage |
|-----------------------|
| 50.00                |

### Explanation:
1. **Customer 1**:
   - First order: `delivery_id = 1` (scheduled).
2. **Customer 2**:
   - First order: `delivery_id = 2` (immediate).
3. **Customer 3**:
   - First order: `delivery_id = 5` (scheduled).
4. **Customer 4**:
   - First order: `delivery_id = 7` (immediate).

- Out of 4 customers, 2 have immediate first orders.
- Percentage of immediate orders: \( \frac{2}{4} \times 100 = 50.00\% \).
## Solution

```sql
SELECT 
  round(
    100 -(
      sum(
        CASE WHEN delivery.order_date != delivery.customer_pref_delivery_date THEN 1 else 0 END
      )/ count(*)
    )* 100, 
    2
  ) as immediate_percentage 
from 
  delivery 
WHERE 
  order_date = (
    SELECT 
      MIN(order_date) 
    FROM 
      delivery d1 
    WHERE 
      d1.customer_id = delivery.customer_id
  )
