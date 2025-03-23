# Constraints

Constraints are rules applied to table columns to enforce the integrity and accuracy of the data stored within the table.
They ensure that only valid data is entered, maintaining the reliability of the database.

## Types of Constraints

### 1. CLC (Column-Level Constraints)
- Applies to a single column.
- Declared alongside the column definition.

### 2. TLC (Table-Level Constraints)
- Can involve one or multiple columns.
- Declared after all column definitions.

---

## 1. Primary Key

- Requires each row to have a **unique** and **non-null** value.
- When a column is declared as a primary key:
  - It is automatically set to `NOT NULL`.
  - An index is created on that column.
- Only **one primary key** is allowed per table.

### CLC Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype CONSTRAINT constraint_name PRIMARY KEY,
    column2 datatype
);
```

### TLC Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
    CONSTRAINT constraint_name PRIMARY KEY (column1, column2, ...)
);
```

### Drop Primary Key Constraint:
```sql
ALTER TABLE table_name
DROP PRIMARY KEY;
```

### Add Primary Key Constraint:
```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
PRIMARY KEY (column_name);
```

---

## 2. Unique Constraints

- Ensure that all values in a column or a combination of columns are unique across the table.
- Can be CLC or TLC.

### Drop UNIQUE Constraint:
```sql
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;
```

### Add UNIQUE Constraint:
```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
UNIQUE (column_name);
```

---

## 3. Foreign Key

- Ensure referential integrity by enforcing that a value in one table matches a value in another table's primary key.
- To create foreign key at the table level we write FOREIGN KEY keywords followed by REFERENCES keyword followed by the name of the related table and the name of the related column in parenthesis.

### Syntax:
```sql
CREATE TABLE child_table (
    column1 datatype,
    column2 datatype,
    -- other columns
    CONSTRAINT constraint_name FOREIGN KEY (column_name) REFERENCES parent_table(parent_column)
);
```

### Drop FOREIGN KEY Constraint:
```sql
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;
```

### Add FOREIGN KEY Constraint:
```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
FOREIGN KEY (child_column)
REFERENCES parent_table(parent_column);
```

---

## Handling Deletions of Primary Table Rows

Oracle provides several options to define how deletions in the parent table affect related rows in the child table:

### 1. ON DELETE CASCADE
- Automatically deletes all child rows that reference the deleted parent row.

#### Use Case:
When you want deletions in the parent table to propagate to child tables, ensuring no orphaned records remain.

#### Syntax Example:
```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL,
    last_name VARCHAR2(50) NOT NULL,
    department_id NUMBER,
    manager_id NUMBER,
    CONSTRAINT fk_department_id FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
        ON DELETE CASCADE,
    CONSTRAINT fk_manager_id FOREIGN KEY (manager_id)
        REFERENCES employees(employee_id)
        ON DELETE CASCADE
);
```

---

### 2. ON DELETE SET NULL
- Sets the foreign key column in child rows to NULL when the referenced parent row is deleted.

#### Use Case:
When you want to retain child records but remove their association with the deleted parent.

#### Syntax Example:
```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL,
    last_name VARCHAR2(50) NOT NULL,
    department_id NUMBER,
    manager_id NUMBER,
    CONSTRAINT fk_department_id FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
        ON DELETE SET NULL,
    CONSTRAINT fk_manager_id FOREIGN KEY (manager_id)
        REFERENCES employees(employee_id)
        ON DELETE SET NULL
);
```

---

### 3. ON DELETE NO ACTION (Default)
- Prevents deletion of a parent row if any child rows reference it.

#### Use Case:
When you want to ensure that parent records cannot be deleted as long as they are referenced by child records.

---

## 4. NOT NULL Constraint

- Ensures that a column cannot have NULL values, enforcing mandatory data entry.
- Not Null can be only CLC.

### Dropping Not Null:
```sql
ALTER TABLE table_name
MODIFY column_name datatype NULL;
```

---

## 5. CHECK Constraint

- Ensures that all values in a column satisfy a specific condition.
- Can be both CLC, TLC.

---

## View Table Constraints
```sql
SELECT constraint_name, constraint_type, status
FROM user_constraints
WHERE table_name = 'MY_TABLE';
```
