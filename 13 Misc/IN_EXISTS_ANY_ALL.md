
We'll use these two sample tables:

---

### 🧾 `students` table:

| id | name   | marks | class |
| -- | ------ | ----- | ----- |
| 1  | Aditi  | 89    | A     |
| 2  | Rohan  | 76    | B     |
| 3  | Sanya  | 91    | A     |
| 4  | Vikram | 68    | C     |

---

### 🧾 `attendance` table:

| student\_id | present\_days |
| ----------- | ------------- |
| 1           | 45            |
| 2           | 50            |
| 4           | 42            |

---

## 🔸 1. `IN` – Check if a value exists in a list

### ✅ Full Query:

```sql
SELECT name
FROM students
WHERE id IN (
    SELECT student_id
    FROM attendance
);
```

### 🔍 Behavior:

* Selects students whose `id` **exists in the attendance table**.
* Result: Aditi, Rohan, Vikram

---

### ⚠️ What if `attendance.student_id` has a NULL?

| student\_id |
| ----------- |
| 1           |
| 2           |
| NULL        |

* Then `IN (1, 2, NULL)` is treated as:

  ```sql
  WHERE id IN (1, 2, NULL)
  ```

* For a row with `id = 3`, SQL says:
  → “Is 3 equal to 1? No.
  Is 3 equal to 2? No.
  Is 3 equal to NULL? UNKNOWN”

* Result: **Row is excluded** due to `NULL` uncertainty (3-valued logic).

---

## 🔸 2. `EXISTS` – Does the subquery return *any* row?

### ✅ Full Query:

```sql
SELECT name
FROM students s
WHERE EXISTS (
    SELECT 1
    FROM attendance a
    WHERE a.student_id = s.id
);
```

### 🔍 Behavior:

* For each student, SQL checks: “Is there any row in attendance where student\_id matches this student?”
* If yes → include.
* Result: Aditi, Rohan, Vikram

---

### ⚠️ What if `attendance.student_id` is NULL?

* Doesn’t matter. `EXISTS` only checks if **any row** meets the condition.
* It ignores the result **values** and cares only about **existence of rows**.

✅ `EXISTS` is **safe with NULLs** and often faster on large data than `IN`.

---

## 🔸 3. `ANY` – Compare to any value in subquery

### ✅ Full Query:

```sql
SELECT name
FROM students
WHERE marks > ANY (
    SELECT marks
    FROM students
    WHERE class = 'A'
);
```

### 🔍 Behavior:

* Compares `marks` against all marks in class A: (89, 91)
* Condition means:
  `marks > 89 OR marks > 91`
* So marks **must be greater than at least one of them**
* Result: Sanya (91 is not > 91), no one else

---

### ⚠️ What if subquery returns NULL?

| marks |
| ----- |
| 89    |
| NULL  |

* `ANY (89, NULL)` still works:

  * As long as **at least one non-NULL** value exists, it compares against it.
  * So: `marks > ANY (89)` → valid.

---

## 🔸 4. `ALL` – Must satisfy condition for **all** values

### ✅ Full Query:

```sql
SELECT name
FROM students
WHERE marks > ALL (
    SELECT marks
    FROM students
    WHERE class = 'C'
);
```

* Class C has 68 → So the condition becomes: `marks > 68`

### 🔍 Behavior:

* Row is selected **only if** its `marks` are greater than **all** returned values.
* Result: Aditi (89), Rohan (76), Sanya (91)

---

### ⚠️ What if subquery returns NULL?

| marks |
| ----- |
| NULL  |

Then the query becomes:

```sql
WHERE marks > ALL (NULL)
```

* SQL can't compare `marks > NULL` → result is `UNKNOWN`
* So **no rows will be returned** ❌

---

## 🧠 Final Comparison

| Clause   | Compares Against   | NULL Safe? | Result if Subquery Has NULL Only            |
| -------- | ------------------ | ---------- | ------------------------------------------- |
| `IN`     | List of values     | ❌ No       | Might exclude rows unexpectedly             |
| `EXISTS` | Row presence       | ✅ Yes      | Works fine                                  |
| `ANY`    | One or more values | ⚠️ Partial | Works **if at least one value is non-NULL** |
| `ALL`    | All values         | ❌ No       | Returns nothing if any NULL present         |

---

## ✅ Summary in English

* Use `IN` for filtering **specific values**.
* Use `EXISTS` to check **presence of related rows**.
* Use `ANY` for “greater than at least one”.
* Use `ALL` for “greater than all”.

---
