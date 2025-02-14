# Capitalize Text in SQL Table

## Table: `user_content`

| Column Name  | Type    |
|-------------|---------|
| content_id  | int     |
| content_text| varchar |

- `content_id` is the unique key for this table.
- Each row contains a unique ID and the corresponding text content.

## Problem Statement

Write a solution to transform the text in the `content_text` column by applying the following rules:

1. **Convert the first letter of each word to uppercase** and the remaining letters to lowercase.
2. **Special handling for words containing special characters:**
   - For words connected with a hyphen (`-`), both parts should be capitalized (e.g., `top-rated` → `Top-Rated`).
3. **All other formatting and spacing should remain unchanged.**

The result table should include both the **original content_text** and the **modified text** following the above rules.

---

## **Example**

### **Input:**

#### `user_content` table:

| content_id | content_text                    |
|------------|---------------------------------|
| 1          | hello world of SQL              |
| 2          | the QUICK-brown fox             |
| 3          | modern-day DATA science         |
| 4          | web-based FRONT-end development |

### **Output:**

| content_id | original_text                   | converted_text                  |
|------------|---------------------------------|---------------------------------|
| 1          | hello world of SQL              | Hello World Of Sql              |
| 2          | the QUICK-brown fox             | The Quick-Brown Fox             |
| 3          | modern-day DATA science         | Modern-Day Data Science         |
| 4          | web-based FRONT-end development | Web-Based Front-End Development |

---

## **Explanation:**

- **For `content_id = 1`**  
  - Each word’s first letter is capitalized: `"Hello World Of Sql"`

- **For `content_id = 2`**  
  - The hyphenated word `"QUICK-brown"` becomes `"Quick-Brown"`
  - Other words follow normal capitalization rules.

- **For `content_id = 3`**  
  - `"modern-day"` becomes `"Modern-Day"`
  - `"DATA"` is converted to `"Data"`.

- **For `content_id = 4`**  
  - `"web-based"` → `"Web-Based"`
  - `"FRONT-end"` → `"Front-End"`

---

## **Python Solution using Pandas**

```python
import pandas as pd

def capitalize_content(user_content: pd.DataFrame) -> pd.DataFrame:
    def capitalize_text(text):
        # Split by spaces and process each word separately
        words = text.split()
        capitalized_words = []
        for word in words:
            # Capitalize each part of a hyphenated word
            capitalized_word = '-'.join([part.capitalize() for part in word.split('-')])
            capitalized_words.append(capitalized_word)        
        return ' '.join(capitalized_words)
    
    # Rename column to match expected output
    user_content = user_content.rename(columns={"content_text": "original_text"})
    user_content["converted_text"] = user_content["original_text"].apply(capitalize_text)
    
    return user_content
