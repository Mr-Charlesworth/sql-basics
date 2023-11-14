# Databases 2:

#### The SeQueL

- SQL is a standard language for storing, manipulating and querying data in relational databases.
- Relational databases store data in tables.
- Each table contains a set of fields (or columns) for types of data.
- Each entity in a table makes up a row of the table.
- MySQL is a database management system (DBMS), it is the software that accepts SQL commands to process the stored data.



## SQL Basics

- These notes go through the basic statements
- Like everything else, don't worry about memorising them,
  just [a reference like w3 schools](https://www.w3schools.com/mysql/)
- Note that the syntax (or dialect) is slightly different for each DBMS
- We are using MySQL, so we are using the MySQL dialect

### Creating a DataBase

- The Following statements create a database called Library then sets the new database as the current database
- Any Statements after this will be run on the current database called 'Library'

```mysql
CREATE DATABASE Library;
USE Library;
```

### Creating a Table

- Now we need to define a schema for the database
- As mentioned, the schema is described in tables

```mysql
CREATE TABLE Books (
Book_id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
title varchar(100),
synopsis varchar(100),
category_id int
);
```

- 'CREATE' and 'TABLE' are keywords, we have created a table called 'Book'
- The 'Book_id' field is
  - of type 'int'
  - must be 'NOT NULL'
  - is the PRIMARY KEY meaning it must be unique for each row in the table
  - will AUTO_INCREMENT meaning that the DBMS will generate new ids for each new row
- The 'title' and 'synopsis' fields are of type varchar (or a string) of size 100 bytes
- The 'category_id' field is of type int, it will store the id for an entry in another 'Category' table

### Adding and Querying Data

- A SELECT statement can get data from the database
- The general form is:

```mysql
SELECT <column_names> FROM <table_name> WHERE <condition>;
```

- We use column names to select the data we want from each returned row
- We can alternatively select all column data with SELECT *

```mysql
SELECT * FROM Book;
```

- But there's no data yet!!
- Before we start adding data, let's make another table to store categories.
- Lets also drop (remove) and remake the book table so that we can see how a FOREIGN KEY constraint is added

```mysql
DROP TABLE Book;
CREATE TABLE Category (
category_id int PRIMARY KEY AUTO_INCREMENT,
title varchar(100)
);
CREATE TABLE Book (
Book_id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
title varchar(100),
synopsis varchar(100),
category_id int,
FOREIGN KEY (category_id) REFERENCES Category(category_id)
);
```

- This time when we create the book table, the final line adds the constraint to say that we want the category_id in the
  book table to act as a foreign key that is referencing the primary key in the Category table (which is also called category_id)
- This means that each book belongs to a category.
- It also means that each category can have many books.
- We should add a category if we want to add a book.

```mysql
INSERT INTO Category (title) VALUES (
‘Fantasy’);

INSERT INTO Book (title,synopsis,category_id) VALUES (
‘The Colour of Magic’, 
‘Rincewind is a wizard, but a poor one at that!’,
1
);
```
- You can use the UPDATE statement to update existing records:
```mysql
UPDATE <table> SET 
<field> = <value>, 
<field> = value>
WHERE <condition>;
```
- Be sure to use the WHERE condition to not update something you shouldn't
```
UPDATE Book SET Synopsis = ‘The main character is an incompetent and cynical Wizard named Rincewind.‘
WHERE book_id = 1;
```
- You can delete entries from a table
```mysql
DELETE FROM <table> 
WHERE
<condition>;
```
- Again, we need to be sure to use the WHERE condition correctly
```mysql
DELETE FROM Book WHERE title like ‘%magic%’
```
- WHERE conditions can be combined using AND, OR and NOT operators
```mysql
WHERE book_id>=1
WHERE title like=‘The%’ AND synopsis like ‘%rincewind%’
WHERE title IN (SELECT title FROM Book_2)
WHERE title like=‘The%’ AND NOT book_id=1
WHERE title like=‘The%’ OR synopsis like ‘%rincewind%’
```

### Joining Data

- When we have defined schemas with tables that are related to one another, we want to join the data together so that we can get the category for each book
- To do this, we can make joining queries
- There are 4 types of join
  - (INNER) JOIN: Returns records that have matching values in both tables 
  - LEFT (OUTER) JOIN: Returns all records from the left table, and the matched records from the right table 
  - RIGHT (OUTER) JOIN: Returns all records from the right table, and the matched records from the left table 
  - FULL (OUTER) JOIN: Returns all records when there is a match in either left or right table

![table-joins](public/images/table-joins.png)

- LEFT JOIN is the most common join we need
```mysql
SELECT <fields>
FROM <table>
LEFT JOIN <other_table> <condition>
```
- In the Books example, where many books have 1 category (or 1 category has many books) we want all the matches from the left (books) table and all matching data from the right table (category)
```mysql
SELECT
	b.book_id,
	b.title as book_title,
	b.synopsis,
	b.category_id,
	c.title as category_title
FROM
	Book b
	LEFT JOIN Category c
	ON b.category_id = c.category_id;
```
## GUI
- It is possible to have too much fun writing SQL into the terminal.
- GUI Tools allow you to do all the functions to have seen here.
- [Try Workbench](https://dev.mysql.com/downloads/workbench/)


