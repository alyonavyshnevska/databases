### Start 

- You need to start MySQL before you can use the mysql command on your terminal. To do this, run 

`brew services start mysql` 

By default, brew installs the MySQL database without a root password. To secure it run: 

`mysql_secure_installation`

To connect run: 

`mysql -uroot` root is the username name here. On my personal mac -uroot works. This connects without a password.

- or  

` mysql -u root -D name_of_database` to select db when logging in

- Add password with ALTER USER 'root'@'localhost' IDENTIFIED BY 'password'; 

- Pop-up: If you want to access your database remotely, you will need an internet access

- Restart mysql

`brew services restart mysql`

- Quit mysql: 

`exit`

- _test_ database is a multi-table employee database to explore


### Data types (at least in MySQL)
- INT  
- DECIMAL(M,N)     M total num of digits in a num, N num of digits after decimal place
- VARCHAR(1  )       string of text of len 1
- BLOB             binary large object. e.g. images or files
- DATE             YYYY-MM-DD
- TIMESTAMP        YYYY-MM-DD HH-MM-SS 


### Commands

- show databases

``` 
SHOW databases;
```

- get the name of the currently connected database 

``` select database() ```

- show tables in currect database

```show tables; ```


- Create database

```CREATE DATABASE name_of_db;```

- Select a database to work with

``` USE name_of_db; ```

- Comment

``` -- this is a comment in MySQL ```

- Create Table: first step to create schema, then insert data

```
CREATE TABLE student (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(30) NOT NULL,
    major VARCHAR(30) DEFAULT 'undecided',    
    PRIMARY KEY (student_id)
); 
``` 

- Get an overview of table

```
DESCRIBE TABLE name
```


- Insert into table

```
-- when inserting values into all columns 
INSERT INTO student VALUES('Kirk'); 
-- only some values, other values NULL or default if default existss
INSERT INTO student(name) VALUES('Claire'); 
```

- Update table 
```
UPDATE student
SET major = 'Undecided', name = 'Tom'
WHERE student_id = 4;
```


- Wildcards  

One character = _ , any num of characters = LIKE '%school%'      

```
SELECT name
FROM movies
WHERE name LIKE '_ove;'

- Like operator: match any movies that begins with Star 

SELECT name  
FROM movies
WHERE name LIKE 'Star%'; 

```

- BETWEEN 

```
SELECT * 
FROM movies
WHERE year BETWEEN 1980 AND 1990
```
- WHERE OR

```
SELECT name
FROM customers
WHERE state = 'ca'
OR state = 'ny'; 
```

- COUNT() takes the name of a column, counts the number of non-empty values

```
SELECT COUNT (branch_id), branch_id
FROM employee;
optional: WHERE price = 0
optional: GROUP BY branch_id;   -- prints all unique branch ids and their counts
```

- Min/Max
```
SELECT 
MAX(price)
FROM fake_apps; 
```

- Unique values

```
SELECT DISTINCT branch_id
FROM employee;
```

- Composite primary key: pair (QuestionID,MemberID) that together are unique

```
CREATE TABLE voting (
  QuestionID NUMERIC,
  MemberID NUMERIC,
  PRIMARY KEY (QuestionID, MemberID)
);
```

- FOREIGN KEY to reference a primary key from other table

```
inside CREATE TABLE
FOREIGN KEY(manager_id) REFERENCES employee(empl_id) ON DELETE SET NULL 
-- alternatively: ON DELETE SET CASCADE
```

- Same thing

```
SELECT employee.first_name
FROM employees

and

SELECT first_name
FROM employee 
```


### Union

- Union = combine the results of multiple select statements into one

```
SELECT first_name  
FROM employee
UNION 
SELECT branch_name 
FROM branch;
```

- Good idea to add AS new_column_name to the first statement (SELECT first_name). Otherwise the resulted table has the name of the column from first SELECT statement 

- Only works from same num of columns: no select firstname, lastname ... UNION SELECT branchname

- works for columns with same datatypes 


### JOIN

- combining rows from two or more tables based on a related columns between them: 

- **INNER JOIN**: 
- combine rows only when they have a shared column in common
    - only employees whos id's matched the id's in branch.mgr_id were selected 

```
-- find all branches  and the names of their managers 
SELECT employee.emp_id, employee.first_name, branch.branch_name  
FROM employee 
JOIN branch 
ON employee.emp_id = branch.mgr_id;

out: 

+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    102 | Michael    | Scranton    |
|    106 | Josh       | Stamford    |
+--------+------------+-------------+

``` 

- **LEFT JOIN**
- All of the rows from the first column specified will be included

```
SELECT employee.emp_id, employee.first_name, branch.branch_name  
FROM employee 
LEFT JOIN branch 
ON employee.emp_id = branch.mgr_id;

out:

+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    102 | Michael    | Scranton    |
|    106 | Josh       | Stamford    |
|    101 | Jan        | NULL        |
|    103 | Angela     | NULL        |
|    104 | Kelly      | NULL        |
|    105 | Stanley    | NULL        |
|    107 | Andy       | NULL        |
|    108 | Jim        | NULL        |
+--------+------------+-------------+

```

- **RIGHT JOIN**
- Includes all of the rows from right table

```
SELECT employee.emp_id, employee.first_name, branch.branch_name  
FROM employee 
RIGHT JOIN branch 
ON employee.emp_id = branch.mgr_id;

out 

+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    102 | Michael    | Scranton    |
|    106 | Josh       | Stamford    |
|   NULL | NULL       | Buffalo     |
+--------+------------+-------------+
``` 

- **FULL OUTER JOIN**
- grab all of the employees and all of the branches no matter if they met the condition `ON employee.emp_id = branch.mgr_id;` or not
- not possible in mysql


### Nested Queries 
- One query informing another query. We just use the results from one query to get the results from another.

- A subquery is a SELECT statement that is nested within another SELECT statement and which return intermediate results. SQL executes innermost subquery first, then next level is applied to the results of the subquery. 
```
-- Find names of all employees who have sold over 30000 to a single client
-- In works_with we have the emp_id and their total sales, but not their names. In employee we have their names
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id IN (
    SELECT works_with.emp_id
    FROM works_with
    WHERE works_with.total_sales > 30000
);
```

- Find all clients who are handled by the branch that Michael Scott manages. Assume you know Michael's id. 

- Tip: first write the inner loop to find branch that michael manages. then add outer loop to find clients that are handled by that branch 
- IN is used if the return of the inner code returns more than one element. (=Michael manages more than one branch). 
- = is used if the inner statement returns just one element

```
SELECT client.client_name 
FROM client 
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE mgr_id = 102
    LIMIT 1  -- michael may manage more branches 
);
```

### ON DELETE
