---

## 📘 **Section 4: Joins – Combining Data from Multiple Tables**

In real-world databases, data is spread across **multiple related tables**, and joins are how we bring them together.

---

### ✅ What We’ll Cover:

1. `INNER JOIN`
2. `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`
3. `SELF JOIN`
4. `CROSS JOIN`
5. `JOIN ON` vs `USING`
6. Realistic examples & edge cases

---

## 🔹 1. `INNER JOIN` – Return matching rows from both tables

### ✅ Syntax:

```sql
SELECT a.column1, b.column2
FROM tableA a
INNER JOIN tableB b
ON a.common_column = b.common_column;
```

### 🎯 Example:

```sql
SELECT students.name, classes.class_name
FROM students
INNER JOIN classes
ON students.class = classes.class_code;
```

### 💡 Use Case:

When you only want data that **exists in both tables**.

---

## 🔹 2. `LEFT JOIN` – All from Left + Matches from Right

## 🔹 `RIGHT JOIN` – All from Right + Matches from Left

### ✅ Example (LEFT JOIN):

```sql
SELECT students.name, classes.class_name
FROM students
LEFT JOIN classes
ON students.class = classes.class_code;
```

### 💡 Use Case:

* `LEFT JOIN`: Show **all students**, even if their class is missing in `classes`.
* `RIGHT JOIN`: Show **all classes**, even if no students are enrolled.

---

## 🔹 3. `FULL OUTER JOIN` – Everything from both tables

```sql
SELECT a.*, b.*
FROM tableA a
FULL OUTER JOIN tableB b
ON a.id = b.id;
```

### 💡 Use Case:

Get all data, even unmatched rows from both tables.
⚠️ Not available in MySQL directly—must use `UNION` of `LEFT` + `RIGHT`.

## `FULL OUTER JOIN using UNION`

```sql
SELECT s.name, c.class_name
FROM students s
LEFT JOIN classes c ON s.class_id = c.class_id

UNION

SELECT s.name, c.class_name
FROM students s
RIGHT JOIN classes c ON s.class_id = c.class_id;
```

---

## 🔹 4. `SELF JOIN` – Join a table to itself

### 🎯 Example:

Find students who are in the same class:

```sql
SELECT a.name AS student1, b.name AS student2
FROM students a
JOIN students b
ON a.class = b.class AND a.id < b.id;
```

---

## 🔹 5. `CROSS JOIN` – Every row with every other row (Cartesian Product)

```sql
SELECT *
FROM sizes
CROSS JOIN colors;
```

💡 Use Case: To create combinations (e.g., for e-commerce: size × color).

---

## 🔹 6. `ON` vs `USING`

* `ON` lets you join on any condition:

  ```sql
  ON a.col1 = b.col2
  ```

* `USING` works when the column names are **the same in both tables**:

  ```sql
  USING (id)
  ```

---

## 🧠 Edge Cases

| Scenario                 | Notes                                                                        |
| ------------------------ | ---------------------------------------------------------------------------- |
| Nulls in Join Column     | `INNER JOIN` skips, `LEFT JOIN` keeps left rows                              |
| Duplicates in Join Key   | Results in Cartesian-like duplicates                                         |
| Multi-column joins       | You can join using multiple conditions: `ON a.id = b.id AND a.type = b.type` |
| Using aliases (`a`, `b`) | Helps when joining same table or for clarity                                 |

---

## ✅ Summary Table

| Join Type         | Keeps Unmatched  | Use Case                               |
| ----------------- | ---------------- | -------------------------------------- |
| `INNER JOIN`      | ❌ (only matched) | Most common, match in both             |
| `LEFT JOIN`       | ✅ Left side      | Keep all from left, matched from right |
| `RIGHT JOIN`      | ✅ Right side     | Keep all from right, matched from left |
| `FULL OUTER JOIN` | ✅ Both sides     | Everything                             |
| `SELF JOIN`       | N/A              | Compare rows within the same table     |
| `CROSS JOIN`      | N/A              | All combinations                       |

---


