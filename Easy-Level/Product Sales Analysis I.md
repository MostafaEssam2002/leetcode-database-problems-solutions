# SQL Solution for Reporting Product Name, Year, and Price of Sales

## Problem Description
We are tasked to report the `product_name`, `year`, and `price` for each sale using two tables:

### 1. Sales Table
| Column Name | Type  | Description                                         |
|-------------|-------|-----------------------------------------------------|
| sale_id     | int   | Unique identifier for each sale (part of primary key). |
| product_id  | int   | Foreign key referencing the `Product` table.         |
| year        | int   | Year in which the sale occurred.                    |
| quantity    | int   | Quantity sold in this sale.                         |
| price       | int   | Price per unit of the product sold in this sale.    |

### 2. Product Table
| Column Name  | Type    | Description                           |
|--------------|---------|---------------------------------------|
| product_id   | int     | Primary key for the product.          |
| product_name | varchar | Name of the product.                 |

We need to retrieve the following fields:
- `product_name`: Name of the product sold.
- `year`: Year of the sale.
- `price`: Price per unit of the product.

---

## Input Example

### Sales Table:
| sale_id | product_id | year | quantity | price |
|---------|------------|------|----------|-------|
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |

### Product Table:
| product_id | product_name |
|------------|--------------|
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |

### Expected Output:
| product_name | year  | price |
|--------------|-------|-------|
| Nokia        | 2008  | 5000  |
| Nokia        | 2009  | 5000  |
| Apple        | 2011  | 9000  |

---

## Solution 1: Using Explicit `INNER JOIN`

```sql
SELECT 
    Product.product_name, 
    Sales.year, 
    Sales.price 
FROM 
    Sales 
JOIN 
    Product 
ON 
    Sales.product_id = Product.product_id;
---
## Solution 2: Using JOIN Without Explicit Syntax
```sql
SELECT 
    product_name, 
    year, 
    price 
FROM 
    Sales, 
    Product 
WHERE 
    Sales.product_id = Product.product_id;

