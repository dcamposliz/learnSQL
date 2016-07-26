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

	




mysql -u root -p
enter password:

CREATE DATABASE test3;
SHOW DATABASES;

USE test3;
SELECT DATABASE();
DROP DATABASE IF EXISTS test3;