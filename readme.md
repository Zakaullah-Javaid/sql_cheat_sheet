## 1. Create Database

CREATE DATABASE myDb;

## 2. Use Database

USE myDb;

## 3. Drop Database

DROP DATABASE myDb;

## 4. Set Database to Readonly

- To make modifications, set READ_ONLY = 0
  ALTER DATABASE myDb READ_ONLY = 1;

## 5. Create Table

CREATE TABLE employees (
employee_id INT,
first_name VARCHAR(50),
last_name VARCHAR(50),
hourly_pay DECIMAL(5, 2),
pay_date DATE
);

## 6. Select From Table

- Consider specifying columns instead of _
  SELECT _ FROM employees;

## 7. Rename Table

- Use ALTER TABLE to rename a table
  ALTER TABLE employees RENAME TO workers;

## 8. Drop Table

DROP TABLE employees;

## 9. Alter Table - Add Column

ALTER TABLE employees
ADD phone_number VARCHAR(15);

## 10. Rename Column

ALTER TABLE employees
RENAME COLUMN phone_number TO email;

## 11. Modify Column Datatype

- Consider using ALTER COLUMN for datatype modification
  ALTER TABLE employees
  MODIFY email VARCHAR(100);

## 12. Change Position of Column

ALTER TABLE employees
MODIFY email VARCHAR(100) AFTER last_name;

## 13. Move Column to First Position

ALTER TABLE employees
MODIFY email VARCHAR(100) FIRST;

## 14. Drop Column From Table

ALTER TABLE employees
DROP COLUMN email;

## 15. Insert Rows

- For multiple rows, use commas to separate each set of values
  INSERT INTO employees
  VALUES (1, "Zaka", "Ullah", 25.10, "2023-02-14"),
  (2, "John", "Doe", 20.0, "1990-05-15");
- Add more rows as needed

## 16. WHERE CLAUSE

SELECT \* FROM employees
WHERE first_name = "Zaka";

SELECT \* FROM employees
WHERE hourly_pay > 20;

SELECT \* FROM employees
WHERE employee_id <> 1;

## OR

SELECT \* FROM employees
WHERE employee_id != 1;

## IS NULL OR IS NOT NULL

SELECT \* FROM employees
WHERE hourly_pay IS NOT NULL;

## 17. Update Data

UPDATE employees
SET hourly_pay = 23.15,
last_name = "Tomato Ketchup"
WHERE employee_id = 11;

## 18. Delete Data

DELETE FROM employees
WHERE employee_id = 11;

## 19. Auto-commit, Commit, and Rollback

- By default, Auto-commit mode is ON
- If accidentally deleting all rows from the 'employees' table, you may want to undo the changes.

## 1. Set Auto-commit OFF: Transactions won't save automatically, and you'll need to manually control each transaction.

SET AUTOCOMMIT = OFF;

## 2. If an accidental DELETE occurs without a WHERE clause, you can Rollback to restore the current transaction.

ROLLBACK; ## This will undo the changes made in the current transaction.

## 3. If you are satisfied with the changes and want to save them, you can Commit.

COMMIT; ## This will save the changes made in the current transaction.

## Remember to set Auto-commit back ON if you want to return to the default behavior.

SET AUTOCOMMIT = ON;

## 20. Create the 'test' table and insert Date AND Time

CREATE TABLE test (
my_date DATE,
my_time TIME,
my_date_time DATETIME
);

## Insert the current date, time, and datetime into the 'test' table

INSERT INTO test (my_date, my_time, my_date_time)
VALUES (CURRENT_DATE(), CURRENT_TIME(), NOW());

## 21. Unique Constraint

CREATE TABLE products (
product_id INT AUTO_INCREMENT PRIMARY KEY,
product_name VARCHAR(25) UNIQUE,
price DECIMAL (4,2) ## total 4 digits and 2 allowed after .
);

## or you can add Unique key constraint by using ALTER

ALTER TABLE products
ADD CONSTRAINT UNIQUE(product_name);

## 22. NOT NULL Constraint

CREATE TABLE products (
product_id INT AUTO_INCREMENT PRIMARY KEY,
product_name VARCHAR(25) UNIQUE,
price DECIMAL(7,2) NOT NULL
);

ALTER TABLE products
MODIFY price DECIMAL(10,2) NOT NULL;

## 23. CHECK Constraint

CREATE TABLE employees (
employee_id INT,
first_name VARCHAR(50),
last_name VARCHAR(50),
hourly_pay DECIMAL(5, 2),
pay_date DATE,
CONSTRAINT check_hourly_pay CHECK (hourly_pay>=10)
);

ALTER TABLE employees
ADD CONSTRAINT cheeck_hourly_pay CHECK (hourly_pay>=10);

ALTER TABLE employees
DROP CHECK check_hourly_pay;

## 24. DEFAULT Constraint

CREATE TABLE products (
product_id INT AUTO_INCREMENT PRIMARY KEY,
product_name VARCHAR(25) UNIQUE,
price DECIMAL(7,2) DEFAULT 0.00
);

ALTER TABLE products
ALTER price SET DEFAULT 0;

## 25. FOREIGN KEY Constraint

CREATE TABLE customers (
customer_id INT AUTO_INCREMENT PRIMARY KEY,
first_name VARCHAR(50),
last_name VARCHAR(50)
);

INSERT INTO customers(first_name,last_name)
VALUES ("Zaka","Ullah"),
("Shahmir","Nazir"),
("Awais","Anwar");

CREATE TABLE transaction (
transaction_id INT AUTO_INCREMENT PRIMARY KEY,
amount DECIMAL (7,2),
customer_id int,
FOREIGN KEY (customer_id) REFERENCES customers (customer_id)
);

ALTER TABLE transaction
DROP FOREIGN KEY transaction_ibfk_1;

ALTER TABLE transaction
ADD CONSTRAINT fk_customer_id ##if table already exist add this line
FOREIGN KEY (customer_id) REFERENCES customers(customer_id);

## 26. JOINS

## Inner Join: Displays matched columns from both tables

-- Use INNER JOIN to combine rows from two tables based on a matching condition. It returns only the rows where there is a match in both tables.

SELECT \*
FROM transaction INNER JOIN customers
ON transaction.customer_id = customers.customer_id;

## Left Join: Displays complete left table and matched right table

-- Use LEFT JOIN to return all rows from the left table and the matching rows from the right table. If there are no matches, NULL values are returned for the columns from the right table.

SELECT \*
FROM transaction LEFT JOIN customers
ON transaction.customer_id = customers.customer_id;

## Right Join: Displays complete right table and matched left table

-- Use RIGHT JOIN to return all rows from the right table and the matching rows from the left table. If there are no matches, NULL values are returned for the columns from the left table.

SELECT \*
FROM transaction RIGHT JOIN customers
ON transaction.customer_id = customers.customer_id;

## 27. Functions

- COUNT Function: Total number of rows in the table
  SELECT COUNT(amount) AS count
  FROM transaction; -- Returns the total number of non-null values in the 'amount' column

- MAX Function: Highest value in the column
  SELECT MAX(amount) as max_amount
  FROM transaction; -- Returns the highest amount in the 'amount' column

- MIN Function: Lowest value in the column
  SELECT MIN(amount) as min_amount
  FROM transaction; -- Returns the lowest amount in the 'amount' column

- AVG Function: Average value in the column
  SELECT AVG(amount) as avg_amount
  FROM transaction; -- Returns the average amount in the 'amount' column

- SUM Function: Sum total of values in the column
  SELECT SUM(amount) as sum_amount
  FROM transaction; -- Returns the sum total of all amounts in the 'amount' column

- CONCAT Function: Concatenates two or more strings
  SELECT CONCAT(first_name," ",last_name) as full_name
  FROM customers; -- Combines 'first_name' and 'last_name' columns to create a full name

## 28. Logical Operator

- Adding a new column to the employees table
  ALTER TABLE employees
  ADD column job VARCHAR(25) AFTER hourly_pay;

- Updating the job for a specific employee
  UPDATE employees
  SET job = "Teacher"
  WHERE employee_id = 19;

- Using logical AND operator to filter records
  SELECT \* FROM employees
  WHERE job = "cook" AND employee_id > 13;

- Using logical NOT operator to filter records
  SELECT \* FROM employees
  WHERE NOT job = "cook" AND NOT job = "Teacher";

- Using BETWEEN operator to filter records within a range
  SELECT \* FROM employees
  WHERE pay_date BETWEEN "2023-02-18" AND "2023-02-25";

- Using IN operator to filter records matching multiple values
  SELECT \* FROM employees
  WHERE job IN ("Guard","Teacher","Doctor");

## 29. Wild Card Characters , (% \_ used to substitute one or more character to string)

- Starts With
  SELECT \* FROM employees
  WHERE first_name LIKE "h%";

- Ends With

SELECT \* FROM employees
WHERE last_name LIKE "%d";

SELECT \* FROM employees
WHERE job LIKE "\_\_ard";

SELECT \* FROM employees
WHERE job LIKE "\_oc%";

## 30. Order By Clause

SELECT \* FROM employees
ORDER BY first_name DESC;

## 31. Limit Clause with OFF-SET

SELECT \* FROM employees
ORDER BY job DESC
LIMIT (3 --ofset),(1 --limit);

## 32. UNION

SELECT first_name, last_name FROM employees
UNION
SELECT first_name,last_name FROM customers;
