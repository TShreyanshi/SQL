## ✅ Aggregate Functions
These functions summarize data across rows.

| Function  | Description             |
| --------- | ----------------------- |
| `COUNT()` | Number of rows          |
| `SUM()`   | Total of numeric column |
| `AVG()`   | Average value           |
| `MIN()`   | Smallest value          |
| `MAX()`   | Largest value           |


🎯 Examples:
```sql
SELECT COUNT(*) FROM students;
SELECT AVG(marks) FROM students;
SELECT MIN(age), MAX(age) FROM students;
```

---

✅ GROUP BY – Group data before aggregation
🔧 Syntax:
```sql
SELECT group_column, AGG_FUNCTION(column)
FROM table
GROUP BY group_column;
```

🎯 Example:
```sql
SELECT class, AVG(marks) AS avg_marks
FROM students
GROUP BY class;
```

💡 Use Case:
Calculate average marks per class.

---


✅ HAVING – Filter after aggregation
🔧 Syntax:
```sql
SELECT group_column, AGG_FUNCTION(column)
FROM table
GROUP BY group_column
HAVING AGG_FUNCTION(column) condition;
```

🎯 Example:
```sql
SELECT class, AVG(marks) AS avg_marks
FROM students
GROUP BY class
HAVING AVG(marks) > 75;
```

💡 Use Case:
Find classes with average marks above 75.


⚠️ Edge Cases

| Situation                              | Solution / Tip                                                     |
| -------------------------------------- | ------------------------------------------------------------------ |
| Using `WHERE` with aggregates          | ❌ Not allowed. Use `HAVING` instead.                               |
| Mixing grouped and non-grouped columns | ❌ You must `GROUP BY` every selected column **except aggregates**. |
| Filtering rows before grouping         | ✅ Use `WHERE` **before** `GROUP BY` to filter data early.          |
| Filtering groups after aggregation     | ✅ Use `HAVING` **after** `GROUP BY`                                |


---


## 📘 Extra Functions Often Used with Aggregates
These are not aggregation functions by themselves, but are commonly used with or around them for better control of output.

🔹 ROUND() – Round a numeric value
✅ Syntax:
```sql
ROUND(number, decimal_places)
```

🎯 Example:
```sql
SELECT ROUND(AVG(marks), 2) AS avg_marks
FROM students;
```

💡 Use Case:
Rounds the average marks to 2 decimal places.

🔹 FLOOR() and CEIL() – Round Down or Up
```sql
SELECT FLOOR(AVG(marks)), CEIL(AVG(marks)) FROM students;
```

💡 Use Case:

```
FLOOR(89.7) → 89
CEIL(89.1) → 90
```

🔹 TRUNC() – Truncate decimals without rounding
(More common in Oracle/PostgreSQL)

```sql
SELECT TRUNC(AVG(marks), 1) FROM students;
```

Unlike ROUND, it cuts the decimals instead of rounding.

🔹 CAST() – Convert data types
```sql
SELECT CAST(AVG(marks) AS INT) FROM students;
```

Use Case: You want integer output instead of float.


