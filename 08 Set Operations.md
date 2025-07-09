## 📘 **SECTION 8: SQL Set Operations**

Set operations combine the results of **two or more `SELECT` queries**.

> 🧠 Think of SQL set operations as **math-style operations on sets of rows**.

---

## ✅ Supported Set Operations

| Operator           | Description                                 | Duplicates |
| ------------------ | ------------------------------------------- | ---------- |
| `UNION`            | Combines results and removes duplicates     | ❌ No dupes |
| `UNION ALL`        | Combines all results including duplicates   | ✅ Yes      |
| `INTERSECT`        | Returns only common rows in both results    | ❌ No dupes |
| `EXCEPT` / `MINUS` | Returns rows from first query not in second | ❌ No dupes |

> 🔸 MySQL supports only `UNION` and `UNION ALL` natively
> 🔸 `INTERSECT` and `EXCEPT` are available in PostgreSQL, SQL Server (or can be emulated in MySQL)

---

## 🧾 Sample Tables

### Table: `science_students`

| id | name  |
| -- | ----- |
| 1  | Aditi |
| 2  | Rohan |
| 3  | Sanya |

---

### Table: `sports_students`

| id | name   |
| -- | ------ |
| 2  | Rohan  |
| 3  | Sanya  |
| 4  | Vikram |

---

---

## 🔹 1. `UNION` – Combine & Remove Duplicates

```sql
SELECT name FROM science_students
UNION
SELECT name FROM sports_students;
```

✅ Output:

| name   |
| ------ |
| Aditi  |
| Rohan  |
| Sanya  |
| Vikram |

➡️ **Duplicates like "Rohan" and "Sanya" appear only once**

---

## 🔹 2. `UNION ALL` – Combine All Rows (with Duplicates)

```sql
SELECT name FROM science_students
UNION ALL
SELECT name FROM sports_students;
```

✅ Output:

| name   |
| ------ |
| Aditi  |
| Rohan  |
| Sanya  |
| Rohan  |
| Sanya  |
| Vikram |

➡️ Keeps all rows, including duplicates

---

## 🔹 3. `INTERSECT` (Not in MySQL)

```sql
SELECT name FROM science_students
INTERSECT
SELECT name FROM sports_students;
```

✅ Output (in PostgreSQL):

| name  |
| ----- |
| Rohan |
| Sanya |

➡️ Returns only rows present in **both** tables.

---

### 🛠️ In MySQL? Use INNER JOIN instead:

```sql
SELECT s.name
FROM science_students s
INNER JOIN sports_students sp ON s.name = sp.name;
```

---

## 🔹 4. `EXCEPT` / `MINUS` (Not in MySQL)

```sql
SELECT name FROM science_students
EXCEPT
SELECT name FROM sports_students;
```

✅ Output (in PostgreSQL):

| name  |
| ----- |
| Aditi |

➡️ Returns rows in the first query that **aren’t in** the second.

---

### 🛠️ In MySQL? Use LEFT JOIN + IS NULL:

```sql
SELECT s.name
FROM science_students s
LEFT JOIN sports_students sp ON s.name = sp.name
WHERE sp.name IS NULL;
```

---

## ⚠️ Rules for Set Operations

| Rule                         | Explanation                                  |
| ---------------------------- | -------------------------------------------- |
| Number of columns must match | Both queries must return same # of columns   |
| Data types must match        | Each corresponding column must be compatible |
| `ORDER BY` goes at the end   | After all unions, not inside each query      |

---

### ❌ Invalid:

```sql
SELECT name FROM science_students ORDER BY name
UNION
SELECT name FROM sports_students;
```

### ✅ Valid:

```sql
SELECT name FROM science_students
UNION
SELECT name FROM sports_students
ORDER BY name;
```

---

## ✅ Summary Table

| Operation   | Supported in MySQL?            | Purpose                          |
| ----------- | ------------------------------ | -------------------------------- |
| `UNION`     | ✅ Yes                          | Combine & remove duplicates      |
| `UNION ALL` | ✅ Yes                          | Combine & keep duplicates        |
| `INTERSECT` | ❌ No (use JOIN)                | Return common rows               |
| `EXCEPT`    | ❌ No (use LEFT JOIN + IS NULL) | Return rows in first, not second |

---
