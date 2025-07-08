## Overview

| Concept       | Keyword           | Example                  | Notes                              |
| ------------- | ----------------- | ------------------------ | ---------------------------------- |
| Selection     | `SELECT`          | `SELECT name FROM users` | Pulls specific columns             |
| Source        | `FROM`            | `FROM users`             | Table name                         |
| Filtering     | `WHERE`           | `WHERE age > 18`         | Filters row-wise                   |
| No Duplicates | `DISTINCT`        | `SELECT DISTINCT dept`   | Unique values                      |
| Sorting       | `ORDER BY`        | `ORDER BY age DESC`      | ASC or DESC                        |
| Limiting      | `LIMIT`, `OFFSET` | `LIMIT 10 OFFSET 5`      | Useful for pagination              |
| Range         | `BETWEEN`         | `BETWEEN 10 AND 20`      | Inclusive                          |
| Multiple      | `IN`              | `IN ('A', 'B')`          | Cleaner than `OR`                  |
| Pattern       | `LIKE`            | `LIKE 'A%'`              | `%` = any chars, `_` = single char |
| Null Check    | `IS NULL`         | `IS NULL`                | Use `IS NULL`, not `= NULL`        |

---


## 📘 SELECT, FROM, WHERE – Core Query Structure
✅ Syntax:

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

🎯 Example:

```sql
SELECT name, age
FROM students
WHERE age > 18;
```

💡 Use Case:
To retrieve specific columns (name, age) from the students table where the age is greater than 18.

⚠️ Exceptional Notes:
<Mark> WHERE works only on rows, not aggregates (use HAVING for that). </Mark>

Case sensitivity depends on the SQL dialect (e.g., MySQL is case-insensitive by default, PostgreSQL is case-sensitive).

</br>

## 📘 DISTINCT – Remove Duplicates
✅ Syntax:
```sql
SELECT DISTINCT column
FROM table_name;
```

🎯 Example:
```sql
SELECT DISTINCT department
FROM employees;
```
💡 Use Case:
To get a list of unique departments from the employees table.

⚠️ Exceptional Notes:
Applies to entire row of columns selected. If you do:

```sql
SELECT DISTINCT name, department
it treats each (name, department) pair as a whole.
```


## 📘 ORDER BY – Sort Results
✅ Syntax:
```sql
SELECT column1
FROM table_name
ORDER BY column1 ASC|DESC;
```

🎯 Example:
```sql
SELECT name, age
FROM students
ORDER BY age DESC;
```

💡 Use Case:
Sort students from oldest to youngest.

⚠️ Exceptional Notes:
Can sort by column alias or column number:

```sql
SELECT name, age FROM students ORDER BY 2 DESC;
```

> NULLs usually come last in ascending and first in descending order (can change in some dialects with NULLS FIRST|LAST).

⚠️ Exceptional Notes:
 This query forces NULL values to appear first, even in ascending order.
```sql
SELECT name, score
FROM students
ORDER BY score ASC NULLS FIRST;
```

## 📘 LIMIT & OFFSET – Paginate Results
✅ Syntax (MySQL/PostgreSQL):
```sql
SELECT column
FROM table_name
LIMIT number OFFSET start;
```

🎯 Example:
```sql
SELECT name FROM students LIMIT 5 OFFSET 10;
```

💡 Use Case:
Return 5 students starting from the 11th record (useful for pagination).

⚠️ Exceptional Notes:
SQL Server uses:

```sql
SELECT TOP 10 * FROM table;
```

⚠️ OFFSET-FETCH:

```sql
SELECT *
FROM students
ORDER BY id
OFFSET 10 ROWS
FETCH NEXT 5 ROWS ONLY;
```


## 📘 BETWEEN, IN, LIKE, IS NULL

🔹 BETWEEN – Range Check (Inclusive)
```sql
SELECT * FROM products WHERE price BETWEEN 100 AND 200;
```
Use Case: Filter prices in a range.

Edge Case: Includes both ends (100 and 200).

🔹 IN – Check for Multiple Values
```sql
SELECT * FROM students WHERE class IN ('A', 'B', 'C');
```
Use Case: Cleaner than multiple ORs.

⚠️ Edge Case: Can use a subquery:
```sql
WHERE id IN (SELECT student_id FROM registrations);
```

🔹 LIKE – Pattern Matching
```sql
SELECT * FROM users WHERE name LIKE 'A%'; -- Starts with A
```
Patterns:

> % = zero or more characters

> _ = exactly one character

⚠️ Edge Case: Case sensitivity depends on DB (PostgreSQL = sensitive, MySQL = insensitive).

🔹 IS NULL / IS NOT NULL
```sql
SELECT * FROM users WHERE phone IS NULL;
```

Use Case: To check for missing values.

> Edge Case: = NULL won’t work. Must use IS NULL.
