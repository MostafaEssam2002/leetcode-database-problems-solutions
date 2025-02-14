## Table: DailySales

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| date_id     | date    |
| make_name   | varchar |
| lead_id     | int     |
| partner_id  | int     |
+-------------+---------+
```
There is no primary key (column with unique values) for this table. It may contain duplicates.
This table contains the date and the name of the product sold and the IDs of the lead and partner it was sold to.
The name consists of only lowercase English letters.

### Task
For each `date_id` and `make_name`, find the number of distinct `lead_id`'s and distinct `partner_id`'s.

Return the result table in any order.

### Example

#### Input:

```
DailySales table:
+-----------+-----------+---------+------------+
| date_id   | make_name | lead_id | partner_id |
+-----------+-----------+---------+------------+
| 2020-12-8 | toyota    | 0       | 1          |
| 2020-12-8 | toyota    | 1       | 0          |
| 2020-12-8 | toyota    | 1       | 2          |
| 2020-12-7 | toyota    | 0       | 2          |
| 2020-12-7 | toyota    | 0       | 1          |
| 2020-12-8 | honda     | 1       | 2          |
| 2020-12-8 | honda     | 2       | 1          |
| 2020-12-7 | honda     | 0       | 1          |
| 2020-12-7 | honda     | 1       | 2          |
| 2020-12-7 | honda     | 2       | 1          |
+-----------+-----------+---------+------------+
```

#### Output:

```
+-----------+-----------+--------------+-----------------+
| date_id   | make_name | unique_leads | unique_partners |
+-----------+-----------+--------------+-----------------+
| 2020-12-8 | toyota    | 2            | 3               |
| 2020-12-7 | toyota    | 1            | 2               |
| 2020-12-8 | honda     | 2            | 2               |
| 2020-12-7 | honda     | 3            | 2               |
+-----------+-----------+--------------+-----------------+
```

### Explanation:
- For `2020-12-8`, `toyota` gets `leads = [0, 1]` and `partners = [0, 1, 2]`, while `honda` gets `leads = [1, 2]` and `partners = [1, 2]`.
- For `2020-12-7`, `toyota` gets `leads = [0]` and `partners = [1, 2]`, while `honda` gets `leads = [0, 1, 2]` and `partners = [1, 2]`.

### SQL Query:

```sql
SELECT date_id, make_name, 
       COUNT(DISTINCT lead_id) AS unique_leads, 
       COUNT(DISTINCT partner_id) AS unique_partners   
FROM DailySales 
GROUP BY date_id, make_name;
```

---

## Steps to Upload to GitHub

1. Save the file as `daily_sales_query.md`.
2. Open a terminal and navigate to the directory containing the file.
3. Initialize a Git repository (if not already done):
   ```sh
   git init
   ```
4. Add the file to the staging area:
   ```sh
   git add daily_sales_query.md
   ```
5. Commit the changes:
   ```sh
   git commit -m "Added SQL query for unique leads and partners by date and make"
   ```
6. Add the remote repository (replace `<your-repo-url>` with your actual GitHub repository URL):
   ```sh
   git remote add origin <your-repo-url>
   ```
7. Push the file to GitHub:
   ```sh
   git push -u origin main
   ```
   (If your branch is `master`, replace `main` with `master`.)
