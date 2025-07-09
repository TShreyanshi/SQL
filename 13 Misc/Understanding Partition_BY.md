
---

## 🔍 Original Query:

```sql
SELECT name, class, marks,
  ROW_NUMBER() OVER (PARTITION BY class ORDER BY marks DESC) AS row_num
FROM students;
```

✅ This assigns row numbers **within each class group**, ordered by `marks DESC`.

---

## 🔍 Modified Query (No `PARTITION BY`):

```sql
SELECT name, class, marks,
  ROW_NUMBER() OVER (ORDER BY marks DESC) AS row_num
FROM students;
```

✅ This assigns **global row numbers** based on `marks DESC` — across the **entire table**, not grouped by class.

---

## 🧾 Example Table: `students`

| id | name   | class | marks |
| -- | ------ | ----- | ----- |
| 1  | Aditi  | A     | 85    |
| 2  | Rohan  | A     | 95    |
| 3  | Sanya  | B     | 90    |
| 4  | Vikram | B     | 72    |
| 5  | Neha   | B     | 85    |
| 6  | Rahul  | A     | 85    |

---

## 🔢 Output with `PARTITION BY class`:

| name   | class | marks | row\_num |
| ------ | ----- | ----- | -------- |
| Rohan  | A     | 95    | 1        |
| Aditi  | A     | 85    | 2        |
| Rahul  | A     | 85    | 3        |
| Sanya  | B     | 90    | 1        |
| Neha   | B     | 85    | 2        |
| Vikram | B     | 72    | 3        |

➡️ Each class (`A`, `B`) is **partitioned separately**.

---

## 🔢 Output without `PARTITION BY`:

| name   | class | marks | row\_num |
| ------ | ----- | ----- | -------- |
| Rohan  | A     | 95    | 1        |
| Sanya  | B     | 90    | 2        |
| Aditi  | A     | 85    | 3        |
| Neha   | B     | 85    | 4        |
| Rahul  | A     | 85    | 5        |
| Vikram | B     | 72    | 6        |

➡️ `ROW_NUMBER()` is **global**: ranking across **all students** based on `marks DESC`.

---

## ✅ Summary

| Feature          | With `PARTITION BY`   | Without `PARTITION BY` |
| ---------------- | --------------------- | ---------------------- |
| Scope of ranking | Resets for each group | Whole result set       |
| Use case         | Top N per group       | Global top N           |
| Example use      | Topper per class      | Top 3 students overall |

---
