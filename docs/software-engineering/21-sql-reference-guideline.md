---
layout: default
title: sql-reference-guideline
parent: software-engineering
nav_order: 21
---
# SQL Reference Guide
{: .no_toc }


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Basic Concepts

SQL (Structured Query Language) is a standard language for managing relational databases. Key concepts include:

- **Database**: Collection of related tables
- **Table**: Collection of related data entries (rows and columns)
- **Row/Record**: Individual data entry
- **Column/Field**: Data attribute
- **Primary Key**: Unique identifier for each row
- **Foreign Key**: Reference to primary key in another table

## Data Types

### Numeric Types
```sql
INT, INTEGER          -- Whole numbers
DECIMAL(p,s)         -- Fixed-point decimal
NUMERIC(p,s)         -- Fixed-point decimal
FLOAT                -- Floating-point decimal
REAL                 -- Single precision floating-point
DOUBLE PRECISION     -- Double precision floating-point
SMALLINT             -- Small integers
BIGINT               -- Large integers
```

### Character Types
```sql
CHAR(n)              -- Fixed-length string
VARCHAR(n)           -- Variable-length string
TEXT                 -- Variable-length string (unlimited)
NCHAR(n)             -- Fixed-length Unicode string
NVARCHAR(n)          -- Variable-length Unicode string
```

### Date/Time Types
```sql
DATE                 -- Date (YYYY-MM-DD)
TIME                 -- Time (HH:MM:SS)
DATETIME             -- Date and time
TIMESTAMP            -- Date and time with timezone
YEAR                 -- Year value
```

### Other Types
```sql
BOOLEAN              -- True/False values
BLOB                 -- Binary large object
CLOB                 -- Character large object
JSON                 -- JSON data (in supporting databases)
```

## Database and Table Operations

### Database Operations
```sql
-- Create database
CREATE DATABASE database_name;

-- Use database
USE database_name;

-- Drop database
DROP DATABASE database_name;

-- Show databases
SHOW DATABASES;
```

### Table Operations
```sql
-- Create table
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    ...
);

-- Example
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10,2),
    hire_date DATE,
    department_id INT
);

-- Drop table
DROP TABLE table_name;

-- Alter table structure
ALTER TABLE table_name ADD COLUMN column_name datatype;
ALTER TABLE table_name DROP COLUMN column_name;
ALTER TABLE table_name MODIFY COLUMN column_name new_datatype;
ALTER TABLE table_name RENAME TO new_table_name;

-- Show table structure
DESCRIBE table_name;
-- or
SHOW COLUMNS FROM table_name;
```

## Data Manipulation (DML)

### INSERT - Adding Data
```sql
-- Insert single row
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);

-- Insert multiple rows
INSERT INTO table_name (column1, column2, column3)
VALUES 
    (value1a, value2a, value3a),
    (value1b, value2b, value3b),
    (value1c, value2c, value3c);

-- Insert from another table
INSERT INTO table_name (column1, column2)
SELECT column1, column2 FROM another_table WHERE condition;
```

### UPDATE - Modifying Data
```sql
-- Update specific rows
UPDATE table_name 
SET column1 = value1, column2 = value2
WHERE condition;

-- Update all rows
UPDATE table_name 
SET column1 = value1;

-- Update with subquery
UPDATE table_name 
SET column1 = (SELECT value FROM another_table WHERE condition)
WHERE condition;
```

### DELETE - Removing Data
```sql
-- Delete specific rows
DELETE FROM table_name WHERE condition;

-- Delete all rows (but keep table structure)
DELETE FROM table_name;

-- Truncate (faster way to delete all rows)
TRUNCATE TABLE table_name;
```

## Data Querying (SELECT)

### Basic SELECT Syntax
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
GROUP BY column_name
HAVING condition
ORDER BY column_name
LIMIT number;
```

### SELECT Examples
```sql
-- Select all columns
SELECT * FROM employees;

-- Select specific columns
SELECT name, salary FROM employees;

-- Select with alias
SELECT name AS employee_name, salary AS monthly_salary 
FROM employees;

-- Select distinct values
SELECT DISTINCT department_id FROM employees;

-- Select with calculated columns
SELECT name, salary, salary * 12 AS annual_salary 
FROM employees;
```

## Filtering and Conditions

### WHERE Clause Operators
```sql
-- Comparison operators
WHERE salary > 50000
WHERE salary >= 50000
WHERE salary < 50000
WHERE salary <= 50000
WHERE salary = 50000
WHERE salary <> 50000  -- or !=

-- Logical operators
WHERE salary > 50000 AND department_id = 1
WHERE salary > 50000 OR department_id = 1
WHERE NOT department_id = 1

-- Pattern matching
WHERE name LIKE 'John%'      -- Starts with 'John'
WHERE name LIKE '%Smith'     -- Ends with 'Smith'
WHERE name LIKE '%son%'      -- Contains 'son'
WHERE name LIKE 'J_hn'       -- J, any char, hn

-- Range conditions
WHERE salary BETWEEN 40000 AND 80000
WHERE hire_date BETWEEN '2020-01-01' AND '2023-12-31'

-- List conditions
WHERE department_id IN (1, 2, 3)
WHERE department_id NOT IN (1, 2, 3)

-- NULL conditions
WHERE email IS NULL
WHERE email IS NOT NULL
```

### CASE Statements
```sql
SELECT name, salary,
    CASE 
        WHEN salary > 80000 THEN 'High'
        WHEN salary > 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

## Sorting and Grouping

### ORDER BY
```sql
-- Single column sorting
SELECT * FROM employees ORDER BY salary;
SELECT * FROM employees ORDER BY salary DESC;

-- Multiple column sorting
SELECT * FROM employees 
ORDER BY department_id, salary DESC;

-- Sorting by column position
SELECT name, salary FROM employees ORDER BY 2 DESC;
```

### GROUP BY and HAVING
```sql
-- Basic grouping
SELECT department_id, COUNT(*) as employee_count
FROM employees 
GROUP BY department_id;

-- Grouping with aggregate functions
SELECT department_id, 
       COUNT(*) as count,
       AVG(salary) as avg_salary,
       MAX(salary) as max_salary,
       MIN(salary) as min_salary
FROM employees 
GROUP BY department_id;

-- HAVING clause (filtering groups)
SELECT department_id, COUNT(*) as employee_count
FROM employees 
GROUP BY department_id
HAVING COUNT(*) > 5;
```

## Joins

### INNER JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

### LEFT JOIN (LEFT OUTER JOIN)
```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

### RIGHT JOIN (RIGHT OUTER JOIN)
```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```

### FULL OUTER JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

### CROSS JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

### SELF JOIN
```sql
SELECT e1.name as employee, e2.name as manager
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.id;
```

### Multiple Joins
```sql
SELECT e.name, d.department_name, l.city
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
INNER JOIN locations l ON d.location_id = l.id;
```

## Subqueries

### WHERE Subqueries
```sql
-- Single value subquery
SELECT name FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Multiple value subquery
SELECT name FROM employees 
WHERE department_id IN (
    SELECT id FROM departments WHERE location_id = 1
);

-- EXISTS subquery
SELECT name FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.employee_id = e.id
);
```

### FROM Subqueries (Derived Tables)
```sql
SELECT dept_summary.department_id, dept_summary.avg_salary
FROM (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
) as dept_summary
WHERE dept_summary.avg_salary > 60000;
```

### SELECT Subqueries (Scalar Subqueries)
```sql
SELECT name, salary,
    (SELECT AVG(salary) FROM employees) as company_avg
FROM employees;
```

### Correlated Subqueries
```sql
SELECT name, salary
FROM employees e1
WHERE salary > (
    SELECT AVG(salary) 
    FROM employees e2 
    WHERE e2.department_id = e1.department_id
);
```

## Functions

### Aggregate Functions
```sql
COUNT(*)              -- Count all rows
COUNT(column)         -- Count non-null values
SUM(column)           -- Sum of values
AVG(column)           -- Average of values
MAX(column)           -- Maximum value
MIN(column)           -- Minimum value
```

### String Functions
```sql
UPPER(string)         -- Convert to uppercase
LOWER(string)         -- Convert to lowercase
LENGTH(string)        -- String length
SUBSTR(string, start, length)  -- Extract substring
CONCAT(str1, str2)    -- Concatenate strings
TRIM(string)          -- Remove leading/trailing spaces
REPLACE(string, old, new)  -- Replace text
```

### Numeric Functions
```sql
ROUND(number, decimals)    -- Round number
CEIL(number)              -- Round up
FLOOR(number)             -- Round down
ABS(number)               -- Absolute value
MOD(number, divisor)      -- Modulo operation
POWER(base, exponent)     -- Exponentiation
```

### Date Functions
```sql
NOW()                     -- Current date and time
CURDATE()                 -- Current date
CURTIME()                 -- Current time
DATE_ADD(date, INTERVAL value unit)  -- Add to date
DATE_SUB(date, INTERVAL value unit)  -- Subtract from date
DATEDIFF(date1, date2)    -- Difference between dates
DATE_FORMAT(date, format) -- Format date
YEAR(date)                -- Extract year
MONTH(date)               -- Extract month
DAY(date)                 -- Extract day
```

### Conditional Functions
```sql
CASE WHEN condition THEN result ELSE alternative END
IF(condition, true_value, false_value)  -- MySQL
COALESCE(value1, value2, ...)  -- First non-null value
NULLIF(value1, value2)         -- NULL if values equal
```

## Indexes

### Creating Indexes
```sql
-- Single column index
CREATE INDEX idx_employee_name ON employees(name);

-- Multiple column index
CREATE INDEX idx_dept_salary ON employees(department_id, salary);

-- Unique index
CREATE UNIQUE INDEX idx_employee_email ON employees(email);

-- Partial index (with condition)
CREATE INDEX idx_high_salary ON employees(salary) WHERE salary > 50000;
```

### Managing Indexes
```sql
-- Drop index
DROP INDEX idx_employee_name ON employees;

-- Show indexes
SHOW INDEX FROM employees;
```

## Constraints

### Primary Key
```sql
-- Single column primary key
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- Composite primary key
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

### Foreign Key
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

-- With cascade options
ALTER TABLE employees 
ADD CONSTRAINT fk_employee_dept 
FOREIGN KEY (department_id) REFERENCES departments(id)
ON DELETE CASCADE ON UPDATE CASCADE;
```

### Other Constraints
```sql
-- NOT NULL
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

-- UNIQUE
CREATE TABLE employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);

-- CHECK constraint
CREATE TABLE employees (
    id INT PRIMARY KEY,
    salary DECIMAL(10,2) CHECK (salary > 0),
    age INT CHECK (age >= 18 AND age <= 65)
);

-- DEFAULT
CREATE TABLE employees (
    id INT PRIMARY KEY,
    hire_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'Active'
);
```

## Views

### Creating Views
```sql
-- Simple view
CREATE VIEW active_employees AS
SELECT id, name, email, salary
FROM employees
WHERE status = 'Active';

-- Complex view with joins
CREATE VIEW employee_details AS
SELECT e.name, e.salary, d.department_name, l.city
FROM employees e
JOIN departments d ON e.department_id = d.id
JOIN locations l ON d.location_id = l.id;
```

### Using Views
```sql
-- Query a view like a table
SELECT * FROM active_employees WHERE salary > 50000;
```

### Managing Views
```sql
-- Modify view
CREATE OR REPLACE VIEW active_employees AS
SELECT id, name, email, salary, hire_date
FROM employees
WHERE status = 'Active';

-- Drop view
DROP VIEW active_employees;
```

## Stored Procedures and Functions

### Stored Procedures
```sql
-- Create stored procedure
DELIMITER //
CREATE PROCEDURE GetEmployeesBySalary(IN min_salary DECIMAL(10,2))
BEGIN
    SELECT name, salary, department_id
    FROM employees
    WHERE salary >= min_salary
    ORDER BY salary DESC;
END //
DELIMITER ;

-- Call stored procedure
CALL GetEmployeesBySalary(50000);

-- Procedure with OUT parameter
DELIMITER //
CREATE PROCEDURE GetEmployeeCount(OUT emp_count INT)
BEGIN
    SELECT COUNT(*) INTO emp_count FROM employees;
END //
DELIMITER ;
```

### User-Defined Functions
```sql
-- Create function
DELIMITER //
CREATE FUNCTION CalculateBonus(salary DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
READS SQL DATA
DETERMINISTIC
BEGIN
    DECLARE bonus DECIMAL(10,2);
    IF salary > 80000 THEN
        SET bonus = salary * 0.10;
    ELSEIF salary > 50000 THEN
        SET bonus = salary * 0.05;
    ELSE
        SET bonus = salary * 0.02;
    END IF;
    RETURN bonus;
END //
DELIMITER ;

-- Use function
SELECT name, salary, CalculateBonus(salary) as bonus
FROM employees;
```

## Transactions

### Basic Transaction Control
```sql
-- Start transaction
START TRANSACTION;
-- or
BEGIN;

-- Commit changes
COMMIT;

-- Rollback changes
ROLLBACK;
```

### Transaction Example
```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- Check if both updates succeeded
IF @@ERROR = 0
    COMMIT;
ELSE
    ROLLBACK;
```

### Savepoints
```sql
START TRANSACTION;

INSERT INTO employees (name, salary) VALUES ('John Doe', 50000);
SAVEPOINT sp1;

UPDATE employees SET salary = 55000 WHERE name = 'John Doe';
SAVEPOINT sp2;

-- Rollback to savepoint
ROLLBACK TO SAVEPOINT sp1;

COMMIT;
```

## Advanced Topics

### Window Functions
```sql
-- ROW_NUMBER
SELECT name, salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- RANK and DENSE_RANK
SELECT name, salary,
    RANK() OVER (ORDER BY salary DESC) as rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank
FROM employees;

-- Partition by
SELECT name, salary, department_id,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) as dept_rank
FROM employees;

-- Running totals
SELECT name, salary,
    SUM(salary) OVER (ORDER BY hire_date ROWS UNBOUNDED PRECEDING) as running_total
FROM employees;
```

### Common Table Expressions (CTE)
```sql
-- Simple CTE
WITH high_earners AS (
    SELECT name, salary, department_id
    FROM employees
    WHERE salary > 80000
)
SELECT he.name, he.salary, d.department_name
FROM high_earners he
JOIN departments d ON he.department_id = d.id;

-- Recursive CTE (organizational hierarchy)
WITH RECURSIVE employee_hierarchy AS (
    -- Base case: top-level managers
    SELECT id, name, manager_id, 1 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case
    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

### UNION Operations
```sql
-- UNION (removes duplicates)
SELECT name FROM employees
UNION
SELECT name FROM contractors;

-- UNION ALL (keeps duplicates)
SELECT name FROM employees
UNION ALL
SELECT name FROM contractors;

-- INTERSECT (common records)
SELECT name FROM employees
INTERSECT
SELECT name FROM contractors;

-- EXCEPT/MINUS (records in first query but not second)
SELECT name FROM employees
EXCEPT
SELECT name FROM contractors;
```

### Pivot Operations
```sql
-- Manual pivot
SELECT 
    department_id,
    SUM(CASE WHEN YEAR(hire_date) = 2020 THEN 1 ELSE 0 END) as hired_2020,
    SUM(CASE WHEN YEAR(hire_date) = 2021 THEN 1 ELSE 0 END) as hired_2021,
    SUM(CASE WHEN YEAR(hire_date) = 2022 THEN 1 ELSE 0 END) as hired_2022
FROM employees
GROUP BY department_id;
```

### Performance Optimization Tips

1. **Use appropriate indexes** on frequently queried columns
2. **Avoid SELECT *** when possible, select only needed columns
3. **Use LIMIT** to restrict result sets
4. **Use EXISTS instead of IN** for subqueries when possible
5. **Avoid functions in WHERE clauses** on large tables
6. **Use appropriate JOIN types** and ensure join conditions use indexes
7. **Analyze query execution plans** to identify bottlenecks
8. **Consider partitioning** for very large tables
9. **Use batch processing** for large data modifications
10. **Normalize database design** appropriately

This reference guide covers the essential SQL concepts and syntax patterns you'll use in most database operations. Keep it handy for quick reference when writing SQL queries!