---

## 📘 **SECTION 9: Window Functions (a.k.a. Analytic Functions)**

Window functions allow you to **perform row-wise calculations across a set (window) of rows** — without collapsing rows like `GROUP BY`.

> 🧠 Think of them as `aggregate + row context` = get `SUM`, `RANK`, etc., *while still seeing every row.*

---

## ✅ Basic Syntax

```sql
SELECT 
  column,
  window_function() OVER (
    PARTITION BY column
    ORDER BY column
  ) AS alias
FROM table;
```

---

## 🧾 Sample Table: `students`

| id | name   | class | marks |
| -- | ------ | ----- | ----- |
| 1  | Aditi  | A     | 85    |
| 2  | Rohan  | A     | 95    |
| 3  | Sanya  | B     | 90    |
| 4  | Vikram | B     | 72    |
| 5  | Neha   | B     | 85    |
| 6  | Rahul  | A     | 85    |

---

## 🔹 1. `ROW_NUMBER()` – Unique row number per group

```sql
SELECT name, class, marks,
  ROW_NUMBER() OVER (PARTITION BY class ORDER BY marks DESC) AS row_num
FROM students;
```

➡️ Gives a **unique row number** per class, ordered by marks.

---

## 🔹 2. `RANK()` – Rankings with ties (gaps exist)

```sql
SELECT name, class, marks,
  RANK() OVER (PARTITION BY class ORDER BY marks DESC) AS class_rank
FROM students;
```

➡️ Same marks get same rank, but skips next ranks (like: 1, 2, 2, 4)

---

## 🔹 3. `DENSE_RANK()` – Rankings without gaps

```sql
SELECT name, class, marks,
  DENSE_RANK() OVER (PARTITION BY class ORDER BY marks DESC) AS class_dense_rank
FROM students;
```

➡️ Same rank for ties, no gaps (like: 1, 2, 2, 3)

---

## 🔹 4. `LAG()` / `LEAD()` – Access previous/next row

```sql
SELECT name, marks,
  LAG(marks) OVER (ORDER BY marks) AS prev_marks,
  LEAD(marks) OVER (ORDER BY marks) AS next_marks
FROM students;
```

➡️ `LAG()` = previous row's value
➡️ `LEAD()` = next row's value

---

## 🔹 5. `SUM()`, `AVG()` etc. as Window Aggregates

```sql
SELECT name, class, marks,
  SUM(marks) OVER (PARTITION BY class) AS total_marks_in_class,
  AVG(marks) OVER (PARTITION BY class) AS avg_marks_in_class
FROM students;
```

➡️ You can use all aggregate functions (`SUM`, `AVG`, `MIN`, `MAX`, `COUNT`) as **windowed aggregates**

---

## 🧠 Key Clauses in `OVER(...)`

| Clause         | Purpose                                  |
| -------------- | ---------------------------------------- |
| `PARTITION BY` | Like GROUP BY — defines logical groups   |
| `ORDER BY`     | Defines row ordering inside partition    |
| `ROWS BETWEEN` | Define a specific frame range (optional) |

---

### 🔸 `ROWS BETWEEN` (Advanced Use)

```sql
SUM(marks) OVER (
  PARTITION BY class 
  ORDER BY marks
  ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
)
```

➡️ Adds marks of previous, current, and next row (moving sum)

---

## ✅ Real-Life Use Cases

| Use Case                      | Function       | Example                           |
| ----------------------------- | -------------- | --------------------------------- |
| Rank students per class       | `RANK()`       | Topper list per class             |
| Track improvement in scores   | `LAG()`        | Compare this test vs previous     |
| Running total                 | `SUM()`        | Cumulative donation or sales      |
| Find students with same marks | `DENSE_RANK()` | Handle ties without skipping rank |
| Difference from class average | `AVG()`        | `marks - avg_marks`               |

---

## ⚠️ Edge Cases & Gotchas

| Case                                 | Behavior                                  |
| ------------------------------------ | ----------------------------------------- |
| Missing `PARTITION BY`               | Applies window across entire table        |
| Missing `ORDER BY` with `RANK()`     | Won’t work — ranks must be ordered        |
| `NULL` in `LEAD()` or `LAG()`        | Returns NULL for first/last rows          |
| Can’t use aliases inside same SELECT | Use subqueries or CTEs for complex chains |

---

## ✅ Summary

| Function           | Purpose                        |
| ------------------ | ------------------------------ |
| `ROW_NUMBER()`     | Unique row numbering per group |
| `RANK()`           | Rank with gaps                 |
| `DENSE_RANK()`     | Rank without gaps              |
| `LAG()` / `LEAD()` | Previous / Next row data       |
| `SUM()`, `AVG()`   | Rolling / grouped aggregations |

---
