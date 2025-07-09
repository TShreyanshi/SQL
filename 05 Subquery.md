
## 📘 **Section 5: Subqueries & Nested SELECTs**

Subqueries are **queries inside other queries** — like Russian dolls 🪆. They're used to filter, calculate, or manipulate data dynamically.

---

### ✅ What You’ll Learn:

1. **Types of Subqueries**

   * Scalar subquery (returns one value)
   * Row subquery (returns one row)
   * Table subquery (returns multiple rows)
2. **WHERE Clause Subqueries**
3. **SELECT Clause Subqueries**
4. **FROM Clause Subqueries (Derived Tables)**
5. **`IN`, `EXISTS`, `ANY`, `ALL`**
6. **Edge Cases & Performance Tips**

---

## 🔹 1. **Subquery in WHERE clause**

### 🎯 Example:

```sql
SELECT name
FROM students
WHERE class_id IN (
    SELECT class_id
    FROM classes
    WHERE class_name = 'Math'
);
```

✅ Use Case:
Get students **in "Math" class**, by first finding the relevant `class_id`.

---

## 🔹 2. **Subquery in SELECT clause (Scalar Subquery)**

Returns **one value per row**.

```sql
SELECT name,
       (SELECT AVG(marks) FROM students) AS avg_marks
FROM students;
```

✅ Use Case:
Add the overall average to every row.

⚠️ Query 2: Aggregate Without GROUP BY
```sql
SELECT name, AVG(marks) AS avg_marks
FROM students;
```
🚨 What this does (or tries to do):

a. You will be mixing:

* A non-aggregated column (name)
* With an aggregate function (AVG(marks))

b. Most SQL dialects will throw an error, like:**

✔️ Correct version if you want per-student average (assuming multiple entries per student):

```sql
SELECT name, AVG(marks) AS avg_marks
FROM students
GROUP BY name;
```

🧠 Use case:
Students have multiple marks (e.g., test scores), and you want to compute average per student.

---

## 🔹 3. **Subquery in FROM clause (Derived Table)**

Use subqueries as **temporary tables**.

```sql
SELECT class, AVG(marks)
FROM (
    SELECT * FROM students WHERE age >= 18
) AS adult_students
GROUP BY class;
```

✅ Use Case:
Filter first, then group.

---

## 🔹 4. **`EXISTS`, `IN`, `ANY`, `ALL`**

### 🔸 `IN`:

Returns true if value exists in a subquery list.

```sql
SELECT name
FROM students
WHERE id IN (
    SELECT student_id
    FROM top_students
);
```

### 🔸 `EXISTS`:

Returns true if subquery returns **at least one row**.

```sql
WHERE EXISTS (SELECT 1 FROM attendance WHERE students.id = attendance.student_id);
```

✅ Faster for large datasets sometimes.

---

### 🔸 `ANY` / `ALL` – Compare with many values

```sql
-- Greater than ANY value
WHERE marks > ANY (SELECT marks FROM students WHERE class = 'A');

-- Greater than ALL values
WHERE marks > ALL (SELECT marks FROM students WHERE class = 'C');
```

---

## ⚠️ Edge Cases & Tips

| Scenario                         | Tip                                                               |
| -------------------------------- | ----------------------------------------------------------------- |
| Subquery returns NULLs           | Be cautious, especially with `IN` / `=` — may behave unexpectedly |
| Subquery returns more than 1 row | Use `IN` / `EXISTS`, not `=` (or filter for single result)        |
| Alias derived tables             | Always use `AS` for subqueries in `FROM` clause                   |
| Performance                      | Avoid unnecessary subqueries; sometimes `JOINs` are faster        |

---

## ✅ Summary Table

| Location      | Type          | Example                                     |
| ------------- | ------------- | ------------------------------------------- |
| `WHERE`       | Filtering     | `WHERE id IN (SELECT id FROM...)`           |
| `SELECT`      | Scalar        | `SELECT (SELECT COUNT(*) FROM...) AS total` |
| `FROM`        | Derived Table | `FROM (SELECT ...) AS sub`                  |
| `EXISTS`      | Existence     | `WHERE EXISTS (SELECT 1 FROM...)`           |
| `ANY` / `ALL` | Comparison    | `WHERE marks > ALL (SELECT ...)`            |

---


