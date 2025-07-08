## Overview 

| Concept       | Keyword     | Example                                      | Notes                             |
| ------------- | ----------- | -------------------------------------------- | --------------------------------- |
| Combine       | `AND`, `OR` | `WHERE age > 17 AND class = 'B'`             | Logical operators                 |
| Negation      | `NOT`       | `WHERE NOT class = 'C'`                      | Opposite condition                |
| If-Else       | `CASE`      | `CASE WHEN marks > 90 THEN 'A' ELSE 'B' END` | Powerful conditional logic        |
| Replace NULL  | `COALESCE`  | `COALESCE(phone, 'No Contact')`              | First non-NULL value              |
| Null if Equal | `NULLIF`    | `NULLIF(class, 'B')`                         | Returns NULL if both inputs match |

---


## 📘 AND, OR, NOT – Combine Conditions
✅ Syntax:
```sql
SELECT * FROM table
WHERE condition1 AND|OR|NOT condition2;
```

🎯 Example:
```sql
SELECT * FROM students
WHERE age > 17 AND class = 'B';
```

💡 Use Case:
- Use AND when both conditions must be true.
- Use OR when either condition is enough.
- Use NOT to negate a condition.

⚠️ Edge Cases:
Parentheses () help with condition clarity:

```sql
WHERE (class = 'B' OR class = 'C') AND age < 18
```
Without them, logic may go wrong due to operator precedence.

---

## 📘 CASE WHEN THEN ELSE END – If-Else Logic

✅ Syntax:
```sql
SELECT name,
       CASE 
           WHEN marks >= 90 THEN 'A'
           WHEN marks >= 75 THEN 'B'
           WHEN marks >= 60 THEN 'C'
           ELSE 'F'
       END AS grade
FROM students;
```

💡 Use Case:
To categorize numerical or text data into labels (grading, status, etc.)

⚠️ Edge Cases:
CASE can be used in SELECT, WHERE, ORDER BY

Can even use CASE inside aggregates:

```sql
SELECT COUNT(CASE WHEN gender = 'F' THEN 1 END) AS females from table;
```

---

## 📘 COALESCE() – First Non-NULL Value

✅ Syntax:
```sql
SELECT name, COALESCE(phone, 'No number') AS contact
FROM students;
```
💡 Use Case:
Replace NULL with a default fallback value.

⚠️ Edge Cases:
Can take multiple arguments, returns the first non-null:

```sql
COALESCE(phone, alt_phone, 'No contact')
```

---

## 📘 NULLIF() – Compare and Return NULL if Equal

✅ Syntax:
```sql
SELECT name, NULLIF(class, 'B') AS filtered_class
FROM students;
```

💡 Use Case:
If class = 'B', it will return NULL; otherwise, return class. Useful in advanced cases like avoiding division by zero:

```sql
SELECT value / NULLIF(divisor, 0) FROM table;
```

⚠️ Edge Cases:
Returns NULL only if the two values match.

