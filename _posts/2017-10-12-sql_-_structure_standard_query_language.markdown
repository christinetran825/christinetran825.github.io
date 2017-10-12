---
layout: post
title:      "SQL - Structure(Standard) Query Language"
date:       2017-10-12 21:59:56 +0000
permalink:  sql_-_structure_standard_query_language
---


How do you pronounce ‘data?’

I say “day-dah”. You say “dad-dah.”

![](https://i.pinimg.com/736x/a5/ed/87/a5ed87d873a2b3042af21d9bcaa35d0a--star-trek-data-star-trek-meme.jpg)

I’m a huge nerd in general. Even little trivia and grains of “did you know…” get me excited. It feeds my child-like awe in everything I encounter. I didn’t expect to have the same reaction when it came to learning about SQL. I like data and charts but I didn’t know what SQL was all about and how much fun it can be.

**SQL is a language for managing data in a database.**

With Ruby, classes are built producing user objects (i.e. users, details, etc…). Storing (persisting) these Ruby objects in a database for future retrieval is important. That’s why SQL and other languages for databases are important.

In general SQL is:
* a relational database
   * databases creates a new table with columns, the columns name, and the type of data to be stored in each column, and the data’s ID.
* stores data in a table structure or spreadsheet with columns and rows
   * column names are in lowercase and uses snake_case (using underscore between words)

**SQL Data Types**

Data types vary between different database systems. SQLite has 4 data types categories:
* TEXT
* INTEGER
* REAL (double precision in other systems) - decimals up to 15 characters long
* BLOB - holds binary data; stored exactly as was input

*Creating a Database and its tables*

``` sqlite3 database_name.db ``` => creates database
```
CREATE TABLE table_name (
   id INTEGER PRIMARY KEY,
   column_name_1 DATATYPE,
   column_name_2 DATATYPE
    ) ;
```

* All tables are defined with ``` id INTEGER PRIMARY KEY ``` column + data type
* All tables are indexed by a number (the id)
* “Primary Key” - data type designation, is unique, and increments (starting at 1) with every new row created.

**SQL QUERY**

Query(Queries) is/are SQL statements that gets data from database(s) (i.e. SELECT)

There are query modifiers who help specify what data gets returned/selected:

* ASC => sort in ascending order
* DESC => sort in descending order
* ORDER BY => order tables by specific SELECT statements; Default is ascending order
* LIMIT => determines the number of records to retrieve from a dataset
* BETWEEN => retrieves a specific data set between two ranges

**SQL Aggregate Functions**

These are functions that gather even more specific data through certain SQL statements. They calculate specific values queried from a database table. Usually getting minimum and maximum values.

*Aggregate Function SQL statements:*

* COUNT => counts the number of records that meet a certain condition
* GROUP BY => groups the results according to the values in a given column

*Aggregate Function Queries for specified values:*

* AVG => returns average value of a column
* SUM
* MIN & MAX
* COUNT => returns number of rows that meet a certain condition
* COUNT (*) = counts rows where at least one column has data

**TABLE RELATIONSHIPS**

SQL works with relational databases (databases that recognizes relations among stored items of info).

In order to create that relationship tables need to have:
* Primary key = always unique
* Foreign key column = uses primary key of another table to refer to a member of that table; acts as point of reference to the other table

Primary Key is the table that’s referenced via the foreign key. It's the parent member that has many of somethings else.

The table that stores members of a table that belongs to another member is the child; it contains the foreign key column

As databases get more complex, the more tables and data will be generated. With SQL JOINS we can link multiple tables in a database together to access values from these tables with a single query. 

There are two types of joining methods for multiple tables:
* Inner join => returns only rows from that database that match the query
* Outer join => returns all of the matching rows AND all of the additional rows from the specified table

We can Imagine Venn Diagrams to help us understand the process:
* Inner join = the overlap
* Left Outer Join = returns all data from left and the overlap
* Right Outer Join = returns all data from the right and the overlap
* Full Outer Join = returns all the data from ENTIRE diagram

OR

we can view it as papers in a 3 ring binder model as described by this BLOG. It’s a very detailed and easy to understand discussion about SQL JOINS - http://blog.seldomatt.com/blog/2012/10/17/about-sql-joins-the-3-ring-binder-model/

INNER JOIN => returns all rows with at least one match in BOTH tables
``` 
SELECT column_name(s)
Optional = AS "table2_table2columnname"
FROM first_table
INNER JOIN second_table
ON first_table.column_name = second_table.column_name;
```

Ex:
```
SELECT Cats.name, Cats.breed, Owners.name  #specify which columns from each table data should be selected from (table_name.column_name)
FROM Cats  #joining tables
INNER JOIN Owners #joining tables
ON Cats.owner_id = Owners.id; #how to join two tables (defining which columns in each table is the foreign key/primary key connection)
```

LEFT JOIN => returns all rows from left table and the matched rows from the right table regardless if they meet the JOIN condition
```
SELECT column_name(s)
FROM first_table
LEFT JOIN second_table
ON first_table.column_name=second_table.column_name;
```

RIGHT JOIN => returns all rows from right table and the matched rows from the left table regardless if they meet the JOIN condition
```
SELECT column_name(s)
FROM first_table
RIGHT JOIN table2
ON first_table.column_name = second_table.column_name;
```

FULL JOIN => returns all rows when there’s a match in ONE of the tables

Outer joins

LEFT OUTER JOIN => returns inner join result and ALL rows from LEFT-MOST(first mentioned) table

RIGHT OUTER JOIN => returns inner join result and ALL rows from RIGHT-MOST (last mentioned) table

FULL OUTER JOIN => returns ALL of the rows from ALL tables


JOINING TABLES

When tables join together, they have common fields through their many-to-many relationship between data

They can be joined like:
``` CREATE TABLE table1name_table2name (...); ```

Inserting data into a joint table
``` INSERT INTO jointablename (table1_id, table2_id) VALUES (data, data); ```

Querying the join table
```
SELECT column(s) #declare column data whose data we want returned
FROM table_one #specify the table whose column being queried
INNER JOIN table_two #joining the table on another table; 
ON table_one.column_name = table_two.column_name
WHERE table_two.column_name = condition;
```

Another aspect of SQL is the the ability to group & sort data

ASC = ascending order
DESC = descending order

ORDER BY() => sorts returned values in ascending order(be default)
```
SELECT column_name, column_name
FROM table_name
ORDER BY column_name ASC|DESC, column_name ASC|DESC;
```

LIMIT => specifies how many of the records results from the query to be returned
``` SELECT * FROM table_name ORDER BY(column_name) DESC LIMIT 1; ```

GROUP BY() => sorts results sets of aggregate functions
ORDER BY() => sorts resulting data set of basic queries
```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

HAVING and WHERE
HAVING = compares aggregates to other values; an additional filter to WHERE clause
WHERE = non-aggregates; can’t be used with aggregates
```
SELECT employee, SUM(bonus) FROM employee_bonus 
GROUP BY employee HAVING SUM(bonus) > 1000;
```


## **FINAL THOUGHTS**

In the end, I view any database as if it were a family tree. How we get information about an individual person or groups of family or any relationships depend on SQL's query methods. There's a lot of code involved with SQL, and I'm sure I'll be learning a more effiecient way of working with databases.

I'll leave you with a fun family tree/database of Harry Potter I found

![](https://i.pinimg.com/736x/ba/c5/bf/bac5bf51208127d1e2c69e6e6d80e0c8--lily-luna-potter-art-lily-luna-potter-and-scorpius-malfoy.jpg)



