
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


---

## 🔍 Query:

```sql
SELECT name, class, marks,
  ROW_NUMBER() OVER (PARTITION BY class ORDER BY marks DESC) AS row_num
FROM students;
```

This uses a **window function** to calculate a new column `row_num` **on-the-fly** — based on class-wise ordering by marks (highest first).

---

## 🔄 What Happens Internally?

This does **NOT** change the actual table.
It simply generates a **temporary result set** — like a view — showing the additional `row_num` column.

---

## 🧾 Suppose `students` table looks like:

| id | name   | class | marks |
| -- | ------ | ----- | ----- |
| 1  | Aditi  | A     | 85    |
| 2  | Rohan  | A     | 95    |
| 3  | Sanya  | B     | 90    |
| 4  | Vikram | B     | 72    |
| 5  | Neha   | B     | 85    |
| 6  | Rahul  | A     | 85    |

---

## 🧾 Output of your query:

| name   | class | marks | row\_num |
| ------ | ----- | ----- | -------- |
| Rohan  | A     | 95    | 1        |
| Aditi  | A     | 85    | 2        |
| Rahul  | A     | 85    | 3        |
| Sanya  | B     | 90    | 1        |
| Neha   | B     | 85    | 2        |
| Vikram | B     | 72    | 3        |

✅ Rows are ranked within each `class`, from highest `marks` down
✅ Ties (like 85 in class A) get **different row numbers**: 2, 3
✅ This column `row_num` is **temporary**, not stored in the table

---

## 🧠 Is the `row_num` permanent?

**No.** It’s a **virtual column**, added only in the result of this query.

If you want to **make it permanent**, you'd have to:

```sql
CREATE VIEW ranked_students AS
SELECT name, class, marks,
  ROW_NUMBER() OVER (PARTITION BY class ORDER BY marks DESC) AS row_num
FROM students;
```

or

```sql
INSERT INTO new_table (name, class, marks, row_num)
SELECT name, class, marks,
  ROW_NUMBER() OVER (PARTITION BY class ORDER BY marks DESC)
FROM students;
```

---
