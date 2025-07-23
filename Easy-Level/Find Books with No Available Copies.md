# Find Fully Borrowed Books in a Library

## 🧾 Table: `library_books`

| Column Name      | Type    |
|------------------|---------|
| book_id          | int     |
| title            | varchar |
| author           | varchar |
| genre            | varchar |
| publication_year | int     |
| total_copies     | int     |

- `book_id` is the unique identifier for this table.
- Each row contains information about a book in the library, including the total number of copies owned by the library.

---

## 🧾 Table: `borrowing_records`

| Column Name   | Type    |
|---------------|---------|
| record_id     | int     |
| book_id       | int     |
| borrower_name | varchar |
| borrow_date   | date    |
| return_date   | date    |

- `record_id` is the unique identifier for this table.
- Each row represents a borrowing transaction.
- `return_date` is `NULL` if the book is currently borrowed and hasn't been returned yet.

---

## 🧠 Problem

Write a solution to find all books that are **currently borrowed** (i.e., `return_date IS NULL`) and have **zero available copies** in the library.

Return the result table ordered by:
1. `current_borrowers` in **descending order**
2. `title` in **ascending order**

---

## 🧪 Example

### Input:

**Table: library_books**

| book_id | title                  | author         | genre     | publication_year | total_copies |
|---------|------------------------|----------------|-----------|------------------|--------------|
| 1       | The Great Gatsby       | F. Scott       | Fiction   | 1925             | 3            |
| 2       | To Kill a Mockingbird  | Harper Lee     | Fiction   | 1960             | 3            |
| 3       | 1984                   | George Orwell  | Dystopian | 1949             | 1            |
| 4       | Pride and Prejudice    | Jane Austen    | Romance   | 1813             | 2            |
| 5       | The Catcher in the Rye | J.D. Salinger  | Fiction   | 1951             | 1            |
| 6       | Brave New World        | Aldous Huxley  | Dystopian | 1932             | 4            |

**Table: borrowing_records**

| record_id | book_id | borrower_name | borrow_date | return_date |
|-----------|---------|----------------|-------------|-------------|
| 1         | 1       | Alice Smith    | 2024-01-15  | NULL        |
| 2         | 1       | Bob Johnson    | 2024-01-20  | NULL        |
| 3         | 2       | Carol White    | 2024-01-10  | 2024-01-25  |
| 4         | 3       | David Brown    | 2024-02-01  | NULL        |
| 5         | 4       | Emma Wilson    | 2024-01-05  | NULL        |
| 6         | 5       | Frank Davis    | 2024-01-18  | 2024-02-10  |
| 7         | 1       | Grace Miller   | 2024-02-05  | NULL        |
| 8         | 6       | Henry Taylor   | 2024-01-12  | NULL        |
| 9         | 2       | Ivan Clark     | 2024-02-12  | NULL        |
| 10        | 2       | Jane Adams     | 2024-02-15  | NULL        |

---

### ✅ Output:

| book_id | title            | author        | genre     | publication_year | current_borrowers |
|---------|------------------|---------------|-----------|------------------|-------------------|
| 1       | The Great Gatsby | F. Scott      | Fiction   | 1925             | 3                 |
| 3       | 1984             | George Orwell | Dystopian | 1949             | 1                 |

---

## ✅ Explanation:

- **The Great Gatsby**  
  Borrowed by 3 people, total copies = 3 → available = 0

- **1984**  
  Borrowed by 1 person, total copies = 1 → available = 0

Books like _To Kill a Mockingbird_, _Pride and Prejudice_, etc., have some available copies, so they are not included.

---

## 💡 SQL Query:

```sql
SELECT 
  library_books.book_id,
  title,
  author,
  genre,
  publication_year,
  COUNT(library_books.book_id) AS current_borrowers
FROM 
  borrowing_records,
  library_books
WHERE 
  borrowing_records.book_id = library_books.book_id
  AND borrowing_records.return_date IS NULL
GROUP BY 
  library_books.book_id, title, author, genre, publication_year, total_copies
HAVING 
  library_books.total_copies - COUNT(library_books.book_id) = 0
ORDER BY 
  current_borrowers DESC,
  library_books.title ASC;
