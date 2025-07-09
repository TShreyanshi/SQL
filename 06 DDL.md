## 📘 **SECTION 6: DDL (Data Definition Language)**

DDL is used to **create, modify, and delete** database objects like:

* Tables
* Databases
* Views
* Indexes
* Constraints

> 🧠 DDL changes the *structure*, not the data. All DDL commands are **auto-committed** — they **cannot be rolled back** in MySQL.

---

## ✅ DDL COMMANDS WITH EXAMPLES

---

### 🔹 1. `CREATE`

#### ➤ a) **Create Table**

```sql
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  marks INT
);
```

➡️ Creates a table named `students` with 4 columns.

---

#### ➤ b) **Create Database**

```sql
CREATE DATABASE school_db;
```

➡️ Creates a new database named `school_db`.

---

### 🔹 2. `ALTER`

Used to **modify table structure**.

#### ➤ a) Add a new column:

```sql
ALTER TABLE students ADD COLUMN phone VARCHAR(15);
```

#### ➤ b) Modify column type:

```sql
ALTER TABLE students MODIFY COLUMN name VARCHAR(150);
```

#### ➤ c) Rename a column:

```sql
ALTER TABLE students RENAME COLUMN phone TO contact_no;
```

#### ➤ d) Drop a column:

```sql
ALTER TABLE students DROP COLUMN contact_no;
```

---

### 🔹 3. `DROP`

Used to **delete a table, column, or database** permanently.

#### ➤ a) Drop a table:

```sql
DROP TABLE students;
```

#### ➤ b) Drop a database:

```sql
DROP DATABASE school_db;
```

⚠️ **Cannot be undone. Deletes data + structure.**

---

### 🔹 4. `TRUNCATE`

Used to **delete all rows** in a table but **keep the table structure**.

```sql
TRUNCATE TABLE students;
```

🔸 Faster than `DELETE FROM students;`
🔸 Cannot be rolled back in MySQL (auto-commit).

---

### 🔹 5. `RENAME`

Rename a table:

```sql
RENAME TABLE students TO learners;
```

---

## 🔐 Constraints in DDL

Used to enforce rules on data. Can be added during `CREATE` or with `ALTER`.

| Constraint    | Purpose                       | Example                                         |
| ------------- | ----------------------------- | ----------------------------------------------- |
| `PRIMARY KEY` | Uniquely identifies each row  | `id INT PRIMARY KEY`                            |
| `UNIQUE`      | Ensures all values are unique | `email VARCHAR(100) UNIQUE`                     |
| `NOT NULL`    | Prevents null values          | `name VARCHAR(100) NOT NULL`                    |
| `DEFAULT`     | Sets a default value          | `age INT DEFAULT 18`                            |
| `CHECK`       | Limits values                 | `CHECK (marks >= 0 AND marks <= 100)`           |
| `FOREIGN KEY` | Link to another table         | `FOREIGN KEY (class_id) REFERENCES classes(id)` |

---

## 🧠 Exceptional & Edge Cases

| Case                               | Behavior                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------- |
| `CREATE TABLE IF NOT EXISTS`       | Avoids error if table already exists                                             |
| `DROP TABLE IF EXISTS`             | Avoids error if table doesn’t exist                                              |
| `ALTER TABLE` on big tables        | Can lock the table temporarily (watch for performance)                           |
| `TRUNCATE` vs `DELETE`             | TRUNCATE is **faster** but **not rollbackable**                                  |
| Dropping a table with foreign keys | May require disabling foreign key checks in MySQL: `SET FOREIGN_KEY_CHECKS = 0;` |

---

## ✅ Summary Table: DDL Commands

| Command    | Description                          | Can be Rolled Back? | Affects Data?    |
| ---------- | ------------------------------------ | ------------------- | ---------------- |
| `CREATE`   | Creates tables, databases, etc.      | ❌ No                | ❌ Structure only |
| `ALTER`    | Modifies table structure             | ❌ No                | ❌                |
| `DROP`     | Deletes entire table or database     | ❌ No                | ✅ Yes            |
| `TRUNCATE` | Deletes all rows but keeps structure | ❌ No                | ✅ Yes            |
| `RENAME`   | Renames tables                       | ❌ No                | ❌                |

---
