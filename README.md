# learnSQL

This document contains notes on SQL. Sources include W3SCHOOLS (http://www.w3schools.com/sql/default.asp) as well as Derek Banas's SQL Tutorial (https://www.youtube.com/watch?v=yPu6qV5byu4).

--

INTRODUCTION

SQL is a standard language for accessing and manipulating databases.

SQL stands for structured query language. SQL is an ANSI (American National Standards Institute) standard.

To build a website that shows data from a database, you need:

 - A RDBMS (relational database management system) database program (e.g. SQL Server, MySQL)

 - A server-side scripting language (e.g. PHP)

 - SQL

 - HTML / CSS

The database in RDBMS is stored in database objects called tables. A table is a collection of related data entries and it consists of columns and rows.

--

SQL SYNTAX

Most of the actions you need to perform on databases are done with SQL statements. 

Here are some of the most important SQL statements:

	SELECT 				- 		extracts data from a database
	UPDATE 				- 		updates data in a database
	DELETE 				- 		deletes data from a database
	INSERT INTO 		-		inserts new data into a database
	CREATE DATABASE 	-		creates a new database
	ALTER DATABASE 		-		modifies a database
	CREATE TABLE 		-		creates a new table
	ALTER TABLE 		-		modifies a table
	DROP TABLE 			-		deletes a table
	CREATE INDEX 		-		creates an index (search key)
	DROP INDEX 			-		deletes an index

--

SELECT

The SELECT statement is used to select data from a database.

	SELECT column_name,column_name 
	FROM table_name;

--

SELECT DISTINCT

The SELECT DISTINCT statement is used to to return only distinct (different) values.

	SELECT DISTINCT column_name,column_name
	FROM table_name;

--

WHERE

The WHERE clause is used to extract only those records that fulfill a specified criterion.

	SELECT column_name,column_name
	FROM table_name
	WHERE column_name operator value;

Example:

	SELECT *
	FROM Customers
	WHERE customerId = 1;

--

OPERATORS IN THE WHERE CLAUSE

	=			Equal
	<>			Not equal. May be written as != .
	>			Greater than
	<			Less than
	>=			Greater than or equal
	<=			Less than or equal
	BETWEEN		Between an inclusive range
	LIKE		Search for a pattern
	IN			To specify multiple possible values for a column

--

AND & OR OPERATORS

 - The AND operator displays a record if both the first condition AND the second condition are true.

 - The OR operator displays a record if either the first condition OR the second condition is true.

EXAMPLES:

	SELECT *
	FROM Customers
	WHERE Country='Germany'
	AND City='Berlin';

	SELECT *
	FROM Customers
	WHERE City='Berlin'
	OR City='London';

--

ORDER BY

The ORDER BY keyword is used to sort the result-set.

	SELECT column_name,column_name
	FROM table_name
	ORDER BY column_name ASC|DESC, column_name ASD|DESC;

Where ASC=ascending and DESC=descending are 'arguments' for the ORDER BY statement. ORDER BY sorts the records in ascending order by default.

EXAMPLES:

	SELECT *
	FROM Customers
	ORDER BY Country;

	SELECT *
	FROM Customers
	ORDER BY Country DESC;

	SELECT *
	FROM Customers
	ORDER BY Country ASC,CustomerName DESC;

--

INSERT INTO

The INSERT INTO statement is used to insert new records in a table.

	INSERT INTO table_name
	VALUES (value1, value2, value3, ...);

	INSERT INTO table_name (column1, column2, column3, ...)
	VALUES (value1, value2, value3, ...);

EXAMPLES

	INSERT INTO Customers(CustomerName, City, Country)
	VALUES ('David', 'Santa Barbara', 'USA');

--

UPDATE

The UPDATE statement is used to update existing records in a table.

	UPDATE table_name
	SET column1 = value1, column2 = value2, ...
	WHERE some_column = some_value;

EXAMPLE:

	UPDATE Customers
	SET ContactName='Alfred Schmidt', City='Hamburg'
	WHERE CustomerName='Alfreds Futterkiste';

When updating records, it's always important to remember to include the WHERE keyword to specify which record to update. Otherwise, all records will update.

--

DELETE

The DELETE statement is used to delete rows in a table.

	DELETE FROM table_name
	WHERE some_column = some_value;

It's important to specify which records to delete by using the WHERE keyword, otherwise all records get deleted.

Example:

	DELETE FROM Customers
	WHERE CustomerName = 'Alfreds Futterkiste' 
	AND ContactName = 'Maria Anders';

	DELETE FROM CUSTOMERS;

	DELETE * FROM CUSTOMERS;

--

INJECTION

SQL Injection is a technique in which malicious users can inject SQL commands into an SQL statment, via web page input.

Example:

 - For an input such as:

  - UserId : ____

  - We entered '105 or 1 = 1', this would run the following query:

	SELECT *
	FROM Users
	WHERE UserId = 105 or 1 = 1;

This will return all rows from the table Users, since 'WHERE 1 = 1' is always true.

There are other ways to hack with a database. To learn more, google 'SQL injections'.

--

SELECT TOP

The SELECT TOP clause is used to specify the number of records to return. It can be useful with large tables with thousands of records - as size can impact performance.

	SELECT TOP number|percent column_name(s)
	FROM table_name;

MySQL SYNTAX:

	SELECT column_name(s)
	FROM table_name
	LIMIT number;

MySQL Example:
	
	SELECT *
	FROM Personas
	LIMIT 5;

--

LIKE

The LIKE operator is used in a WHERE clause to search for a specified pattern in a column.

	SELECT column_name(s)
	FROM table_name
	WHERE column_name LIKE pattern;

Example:

	SELECT *
	FROM Customers
	WHERE City LIKE 's%';

	SELECT *
	FROM Customers
	WHERE City LIKE '%s';

	SELECT *
	FROM Customers
	WHERE Country LIKE '%land%';

The "%" sign is used to define wildcards (missing letters) both before and after the pattern.

--

WILDCARDS

A wildcard character is used to substitute for any other character(s) in a string.

	%				A substitute for zero or more characters
	_				A substitute for a single character
	[charlist]		Sets and ranges of characters to match
	[^charlist]		(Also [!charlist]) Matches only a character not specified within the brackets.

Example:

	SELECT *
	FROM Customers
	WHERE City LIKE '_erlin';

	SELECT *
	FROM Customers
	WHERE City LIKE 'L_n_on';

	SELECT * 
	FROM Customers
	WHERE City 
	LIKE '[a-c]';

	SELECT *
	FROM Customers
	WHERE City NOT LIKE '[bsp]%';

--

IN

The IN operator allows you to identify multiple values in a WHERE clause.

	SELECT column_name
	FROM table_name
	WHERE column_name IN (value1, value2, ...);

Example:

	SELECT *
	FROM Customers
	WHERE City IN ('London','Paris');

--

BETWEEN

The BETWEEN operator is used to select values within a range.

	SELECT column_name(s)
	FROM table_name
	WHERE column_name BETWEEN value1 AND value2;

Examples:

	SELECT *
	FROM Products
	WHERE Price
	BETWEEN 10 AND 20;

	SELECT *
	FROM Products
	WHERE Price
	NOT BETWEEN 10 AND 20;

	SELECT *
	FROM Products
	WHERE (Price BETWEEN 10 AND 20)
	AND NOT CategoryId IN (1,2,3);

	SELECT *
	FROM Products
	WHERE ProductName
	BETWEEN 'C' AND 'M';

--

ALIASES

Aliases are used temporarity to rename a table or a column heading. They are used to make names more readable.

	SELECT column_name
	AS alias_name
	FROM table_name;

	SELECT column_name
	FROM table_name AS alias_name;

Example:

	SELECT CustomerName AS Customer,
	ContactName AS [Contact Person]
	FROM Customers;

--

JOINS

Joins are used to combine rows from two or more tables, based on a common field between them.

The most common type of join is SQL INNER JOIN (simple join). An SQL INNER JOIN returns all rows from multiple tables where the join condition is met.

Example:

	SELECT Orders.OrderID,
	Customers.CustomerName,
	Orders.OrderDate
	FROM Orders
	INNER JOIN Customers
	ON Orders.CustomerID=Customers.CustomerID;

This example refers to two tables (e.g. Orders and Customers), and returns columns OrderID and OrderDate (in table Orders) and column CustomerName (in table Customers) where CustomerID matches across tables Orders and Customers.

There are actually various versions of JOINs.

	INNER JOIN: Returns all rows when there is at least one match in BOTH tables
	LEFT JOIN: Return all rows from the left table, and the matched rows from the right table
	RIGHT JOIN: Return all rows from the right table, and the matched rows from the left table
	FULL JOIN: Return all rows when there is a match in ONE of the tables

--

INNER JOIN

The INNER JOIN keyword selects all rows from both tables as long as there is a match between the columns in both tables.

	SELECT column_name(s)
	FROM table1
	INNER JOIN table2
	ON table1.column_name = table2.column_name;

	SELECT column_name(s)
	FROM table1
	JOIN table2
	ON table1.column_name = table2.column_name;

Think of JOIN or INNER JOIN as the interception of two tables.

Example:
	
	SELECT Customers.CustomerName, Orders.OrderID
	FROM Customers
	INNER JOIN Orders
	ON Customers.CustomerID = Orders.CustomerID
	ORDER BY Customers.CustomerName;

--

LEFT JOIN

The LEFT JOIN keyword (also LEFT OUTER JOIN) returns all keys words from the left table (table1), with the matching rows in the right table (table2). The result is NULL in the right side when there is no match.

	SELECT column_name(s)
	FROM table1
	LEFT JOIN table2
	ON table1.column_name = table2.column_name;

	SELECT column_name(s)
	FROM table1
	LEFT OUTER JOIN table2
	ON table1.column_name = table2.column_name;

Example:

	SELECT Customers.CustomerName, Orders.OrderID
	FROM Customers
	LEFT JOIN Orders
	ON Customers.CustomerID = Orders.CustomerID
	ORDER BY Customers.CustomerName;

--

RIGHT JOIN

The RIGHT JOIN keyword (also RIGHT OUTER JOIN) returns all rows from the right table (table2), with the matchin rows in the left table (table1). The result is NULL in the left side when there is no match.
	
	SELECT column_name(s)
	FROM table1
	RIGHT JOIN table2
	ON table1.column_name = table2.column_name;

	SELECT column_name(s)
	FROM table1
	RIGHT OUTER JOIN table2
	ON table1.column_name = table2.column_name;	

Example:

	SELECT Orders.OrderID,
	Employees.FirstName
	FROM Orders
	RIGHT JOIN Employees
	ON Orders.EmployeeID = Employees.EmployeeID
	ORDER BY Orders.OrderID;

The RIGHT JOIN keyword returns all the rows from the right table (Employees), even if there are no matches in the left table (Orders).

--

FULL OUTER JOIN

The FULL OUTER JOIN keyword returns all rows from the left table (table1) and from the right table (table2). The FULL OUTER JOIN keyword combines the result of both LEFT and RIGHT joins.

	SELECT column_name(s)
	FROM table1
	FULL OUTER JOIN table2
	ON table1.column_name=table2.column_name;

--

UNION

The UNION operator is used to combine the result-set of two or more SELECT statements. Notice that eact SELECT statement within the UNION must have the same number of columns. The columns mist also have similar data types.

	SELECT column_name(s)
	FROM table1
	UNION
	SELECT column_name(s)
	FROM table2;

Example:

	SELECT City
	FROM Customers
	UNION
	SELECT City
	FROM Suppliers
	ORDER BY City;

--

SELECT INTO

The SELECT INTO statement selects data from table and inserts in into a new table.

	SELECT *
	INTO newtable [IN externaldb]
	FROM table1;

	SELECT column_name(s)
	INTO newtable [IN externaldb]
	FROM table1;

Example:

	SELECT *
	INTO CustomersBackUp2013
	FROM Customers;

	SELECT *
	INTO CustomersBackUp2013 IN 'BackUp.mdb'
	FROM Customers;

--

SELECT INTO SELECT

The SELECT INTO SELECT statement copies data from one table and inserts it into an existing table.

	INSERT INTO table2
	SELECT * FROM table1;

	INSERT INTO table2
	(column_name(s))
	SELECT column_name(s)
	FROM table1;

Example:

	INSERT INTO Customers
	(CustomerName, Country)
	SELECT SupplierName, Country 
	FROM Suppliers;

	INSERT INTO Customers
	(CustomerName, Country)
	SELECT SupplierName, Country
	FROM Suppliers
	WHERE Country='Germany';

--

CREATE DATABASE

The CREATE DATABASE statement is used to create a database.

	CREATE DATABASE dbname;

Example:

	CREATE DATABASE myDB;

--

CREATE TABLE

The CREATE TABLE statement is used to create a table in a database.

	CREATE TABLE table_name
	(
		column_name1 data_type(size),
		column_name2 data_type(size),
		column_name3 data_type(size)
	);

The column_name parameters specify the names of the columns of the table.

The data_type parameter specifies what type of data the column can hold (e.g. varchar, integer, decimal, date, ...).

The size parameter specifies the maximum length of the column of the table.

Example:

	CREATE TABLE Persons
	(
	PersonID int,
	FirstName varchar(255),
	LastName varchar(255),
	Address varchar(255),
	City varchar(255)
	);

--

CONSTRAINTS

SQL constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborded by the constraint. Constraints can be specified when the table is created (inside the CREATE TABLE statement) or after the table is created (inside the ALTER TABLE statement).

	CREATE TABLE table_name
	(
	column_name1 data_type(size) constraint_name,
	column_name2 data_type(size) constraint_name,
	column_name3 data_type(size) constraint_name,
	);

In SQL, we have the following constraints:

	NOT NULL 		Indicates that a column cannot store NULL value
	
	UNIQUE 			Ensures that each row for a column must have a unique value
	
	PRIMARY KEY 	A combination of a NOT NULL and UNIQUE. Ensures that a column (or combination of two or more columns) have a unique identity which helps to find a particular record in a table more easily and quickly
	
	FOREIGN KEY 	Ensure the referential integrity of the data in one table to match values in another table
	
	CHECK 			Ensures that the value in a column meets a specific condition
	
	DEFAULT 		Specifies a default value for a column

--

LOGGING INTO MYSQL AS ROOT USER

	mysql -u root -p

--

SHOWING DATABASES

	SHOW DATABASES;

--

USING A DATABASE

	USE database_name;
--

DELETING (DROPPING) DATABASE

	DROP DATABASE IF EXISTS database_name;