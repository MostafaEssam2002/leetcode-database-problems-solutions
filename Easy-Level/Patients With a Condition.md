## Problem Description

### Table: Patients

| Column Name  | Type    |
|--------------|---------|
| patient_id   | int     |
| patient_name | varchar |
| conditions   | varchar |

- `patient_id` is the primary key.
- The `conditions` column contains zero or more codes separated by spaces.
- This table stores information about patients in the hospital.

### Goal
Write a query to find the `patient_id`, `patient_name`, and `conditions` of patients who have Type I Diabetes. Type I Diabetes always starts with the `DIAB1` prefix.

### Output
The result should be returned in any order.

---

## Example

### Input
| patient_id | patient_name | conditions   |
|------------|--------------|--------------|
| 1          | Daniel       | YFEV COUGH   |
| 2          | Alice        |              |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |
| 5          | Alain        | DIAB201      |

### Output
| patient_id | patient_name | conditions   |
|------------|--------------|--------------|
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |

---

## Solution

### Query
```sql
SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions REGEXP '(^| )DIAB1';
