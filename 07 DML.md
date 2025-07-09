
## 📘 **SECTION 7: DML – Data Manipulation Language**

DML is used to **insert, update, or delete** the actual **records (rows)** in your database tables.

> 🧠 DML works on **data**, not structure.
> ✅ These changes **can be rolled back** using `ROLLBACK` (unless auto-commit is ON).

---

## ✅ DML Commands Overview

| Command  | Purpose                   |
| -------- | ------------------------- |
| `INSERT` | Add new data into a table |
| `UPDATE` | Modify existing data      |
| `DELETE` | Remove data from a table  |

---

## 🔹 1. `INSERT` – Add New Rows

### ➤ a) Insert full row:

```sql
INSERT INTO students (id, name, age, marks)
VALUES (1, 'Aditi', 18, 89);
```

### ➤ b) Insert multiple rows:

```sql
INSERT INTO students (id, name, age, marks)
VALUES 
  (2, 'Rohan', 19, 76),
  (3, 'Sanya', 18, 91);
```

### ➤ c) Insert using subquery:

```sql
INSERT INTO top_students (id, name, marks)
SELECT id, name, marks
FROM students
WHERE marks > 90;
```

---

## 🔹 2. `UPDATE` – Modify Existing Rows

### ➤ a) Update a single column:

```sql
UPDATE students
SET marks = 95
WHERE name = 'Rohan';
```

### ➤ b) Update multiple columns:

```sql
UPDATE students
SET age = 20, marks = 90
WHERE id = 2;
```

### ➤ c) Update using subquery:

```sql
UPDATE students
SET marks = marks + 5
WHERE id IN (
  SELECT student_id
  FROM attendance
  WHERE present_days > 40
);
```

---

## 🔹 3. `DELETE` – Remove Rows

### ➤ a) Delete specific rows:

```sql
DELETE FROM students
WHERE age < 18;
```

### ➤ b) Delete all rows (but keep structure):

```sql
DELETE FROM students;
```

🔸 Similar to `TRUNCATE` but **can be rolled back** and **activates triggers**.

---

## ✅ Special Cases & Edge Behaviors

| Scenario                          | Explanation                                   |
| --------------------------------- | --------------------------------------------- |
| `INSERT INTO ... SELECT ...`      | Copy rows between tables                      |
| `UPDATE` without `WHERE`          | Updates all rows in the table ⚠️              |
| `DELETE` without `WHERE`          | Deletes all rows from the table ⚠️            |
| Auto-increment columns            | Can skip in `INSERT`: only pass non-ID fields |
| `ON DUPLICATE KEY UPDATE` (MySQL) | Useful for upserts (insert or update)         |

### ➤ Example:

```sql
INSERT INTO students (id, name, marks)
VALUES (1, 'Aditi', 90)
ON DUPLICATE KEY UPDATE marks = 90;
```

---

## 🧠 Transaction Use with DML

To ensure **safe changes**, wrap DML in a transaction:

```sql
BEGIN;

UPDATE students SET marks = 100 WHERE id = 1;

-- If something goes wrong
ROLLBACK;

-- If all is good
COMMIT;
```

---

## ✅ Summary Table

| Command  | Purpose              | Can Rollback? | Caution Needed          |
| -------- | -------------------- | ------------- | ----------------------- |
| `INSERT` | Add new data         | ✅ Yes         | Check for constraints   |
| `UPDATE` | Change existing data | ✅ Yes         | Be careful with `WHERE` |
| `DELETE` | Remove data          | ✅ Yes         | Don't skip `WHERE`!     |

---
