### Start 

- You need to start MySQL before you can use the mysql command on your terminal. To do this, run 

`brew services start mysql` 

By default, brew installs the MySQL database without a root password. To secure it run: 

`mysql_secure_installation`

To connect run: 

`mysql -uroot` root is the username name here. On my personal mac -uroot works. This connects without a password.

- or  

` mysql -u root -D name_of_database -p ` to select db when logging in

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

