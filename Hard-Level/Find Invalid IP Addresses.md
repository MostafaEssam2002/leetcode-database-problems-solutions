# Table: logs

| Column Name | Type    |
|-------------|---------|
| log_id      | int     |
| ip          | varchar |
| status_code | int     |

`log_id` is the unique key for this table.
Each row contains server access log information including IP address and HTTP status code.

## Problem Statement
Write a solution to find invalid IP addresses. An IPv4 address is invalid if it meets any of these conditions:

- Contains numbers greater than 255 in any octet
- Has leading zeros in any octet (like 01.02.03.04)
- Has less or more than 4 octets

Return the result table ordered by `invalid_count`, `ip` in descending order respectively.

## Example

### Input:

**logs table:**

| log_id | ip            | status_code |
|--------|--------------|-------------|
| 1      | 192.168.1.1   | 200         |
| 2      | 256.1.2.3     | 404         |
| 3      | 192.168.001.1 | 200         |
| 4      | 192.168.1.1   | 200         |
| 5      | 192.168.1     | 500         |
| 6      | 256.1.2.3     | 404         |
| 7      | 192.168.001.1 | 200         |

### Output:

| ip            | invalid_count |
|--------------|--------------|
| 256.1.2.3     | 2            |
| 192.168.001.1 | 2            |
| 192.168.1     | 1            |

### Explanation:

- `256.1.2.3` is invalid because 256 > 255
- `192.168.001.1` is invalid because of leading zeros
- `192.168.1` is invalid because it has only 3 octets

The output table is ordered by `invalid_count`, `ip` in descending order respectively.

## SQL Query

```sql
SELECT ip, COUNT(ip) AS invalid_count 
FROM logs 
WHERE 
    ip NOT REGEXP '^(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\.(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\.(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\.(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])$' 
    OR ip REGEXP '\b0\d' 
    OR LENGTH(ip) - LENGTH(REPLACE(ip, '.', '')) != 3 
GROUP BY ip 
ORDER BY invalid_count DESC, ip DESC;