# Node Type in a Tree

## Problem Description

### Table: Tree

| Column Name | Type |
|-------------|------|
| id          | int  |
| p_id        | int  |

- `id` is the **column with unique values** for this table.
- Each row contains **information about a node ID and its parent ID** in a tree.
- The given structure is **always a valid tree**.

### Node Types:
Each node in the tree can be one of the following three types:

- **"Leaf"**: If the node **has a parent** but **no children**.
- **"Root"**: If the node **has no parent** (`p_id IS NULL`).
- **"Inner"**: If the node **has both a parent and children**.

### Task:
Write a query to **report the type of each node** in the tree.

Return the result table **in any order**.

### Example:

#### Example 1:

##### Input:
**Tree table:**

| id | p_id |
|----|------|
| 1  | NULL |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |

##### Output:

| id | type  |
|----|-------|
| 1  | Root  |
| 2  | Inner |
| 3  | Leaf  |
| 4  | Leaf  |
| 5  | Leaf  |

#### Explanation:
- **Node 1** is the **Root** (it has no parent and has children: **2, 3**).
- **Node 2** is an **Inner node** (it has a parent **1** and children **4, 5**).
- **Nodes 3, 4, and 5** are **Leaf nodes** (they have a parent but no children).

---

#### Example 2:

##### Input:
**Tree table:**

| id | p_id |
|----|------|
| 1  | NULL |

##### Output:

| id | type  |
|----|-------|
| 1  | Root  |

#### Explanation:
- Since **there is only one node**, it is the **Root**.

---

## Solution:

```sql
SELECT id, 
       CASE 
           WHEN p_id IS NULL THEN 'Root' 
           WHEN id IN (SELECT DISTINCT p_id FROM Tree) THEN 'Inner' 
           ELSE 'Leaf' 
       END AS type 
FROM Tree;
