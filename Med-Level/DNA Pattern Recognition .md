# DNA Sequence Analysis

## Table: Samples

| Column Name   | Type    |
|--------------|--------|
| sample_id    | int    |
| dna_sequence | varchar |
| species      | varchar |

- `sample_id` is the unique key for this table.
- Each row contains a DNA sequence represented as a string of characters (`A, T, G, C`) and the species it was collected from.

## Problem Statement

Biologists are studying basic patterns in DNA sequences. Write a solution to identify `sample_id` with the following patterns:

1. Sequences that start with `ATG` (a common start codon).
2. Sequences that end with either `TAA`, `TAG`, or `TGA` (stop codons).
3. Sequences containing the motif `ATAT` (a simple repeated pattern).
4. Sequences that have at least **3 consecutive `G`** (like `GGG` or `GGGG`).

The result should be ordered by `sample_id` in **ascending order**.

---

### Example:

#### **Input Table: Samples**

| sample_id | dna_sequence     | species   |
|-----------|-----------------|-----------|
| 1         | ATGCTAGCTAGCTAA | Human     |
| 2         | GGGTCAATCATC    | Human     |
| 3         | ATATATCGTAGCTA  | Human     |
| 4         | ATGGGGTCATCATAA | Mouse     |
| 5         | TCAGTCAGTCAG    | Mouse     |
| 6         | ATATCGCGCTAG    | Zebrafish |
| 7         | CGTATGCGTCGTA   | Zebrafish |

#### **Expected Output:**

| sample_id | dna_sequence     | species    | has_start | has_stop | has_atat | has_ggg |
|-----------|-----------------|------------|-----------|----------|----------|---------|
| 1         | ATGCTAGCTAGCTAA | Human      | 1         | 1        | 0        | 0       |
| 2         | GGGTCAATCATC    | Human      | 0         | 0        | 0        | 1       |
| 3         | ATATATCGTAGCTA  | Human      | 0         | 0        | 1        | 0       |
| 4         | ATGGGGTCATCATAA | Mouse      | 1         | 1        | 0        | 1       |
| 5         | TCAGTCAGTCAG    | Mouse      | 0         | 0        | 0        | 0       |
| 6         | ATATCGCGCTAG    | Zebrafish  | 0         | 1        | 1        | 0       |
| 7         | CGTATGCGTCGTA   | Zebrafish  | 0         | 0        | 0        | 0       |

---

### **SQL Query:**
```sql
SELECT *, 
    CASE WHEN LEFT(dna_sequence,3) = "ATG" THEN 1 ELSE 0 END AS `has_start`,
    CASE WHEN RIGHT(dna_sequence,3) IN ('TAA', 'TAG', 'TGA') THEN 1 ELSE 0 END AS `has_stop`,
    CASE WHEN dna_sequence REGEXP "ATAT+" THEN 1 ELSE 0 END AS `has_atat`,
    CASE WHEN dna_sequence REGEXP "GGG+" THEN 1 ELSE 0 END AS `has_ggg`
FROM `samples`;
