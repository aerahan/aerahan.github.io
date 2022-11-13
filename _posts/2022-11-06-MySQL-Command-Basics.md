---
title: "MySQL Command Basics"
layout: post
date: 2022-11-06
feature_image: images/mysql_command_basics.png
tags: [mysql, database, basics]
---

MySQL is a popular open source relational database management system. 


Most MySQL commands are self explanatory, so I'll just generally drop an example or syntax with a short explanation. One note, a MySQL statement ends at the semi-colon `;`. 

<!--more-->

<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>

#### Create a Database
To create a database and change to that database:

```sql
-- this is a comment. Ensure there is a space after the two hyphens. 
CREATE DATABASE staff;
USE staff;
```

<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Create a Table within a Database

```sql
CREATE TABLE person(
personID INT UNSIGNED AUTO_INCREMENT,
name VARCHAR(20) NOT NULL,
address VARCHAR(20),
zipCode CHAR(5),
state VARCHAR(20),
dob DATE,
CONSTRAINT person_pk PRIMARY KEY(personID)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

Constraints can be added using `CONSTRAINT`, including a primary key/unique identifier. In this example, `personID` became the primary key. 
`personID` was also set to auto increment, meaning it will start at 1 and continue up.


<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Data Types

Some data types that MySQL accepts include:
- `VARCHAR(20)` - a variable length string; in this case, can contain up to 20 letters
- `CHAR(5)` - a fixed length character/string; in this case, 5
- `INT` - an integer value
- `DATE` - a date, stored as `'yyyy-mm-dd'`
- `DECIMAL(X,Y)` - an integer value of X length with Y digits after the point. (5,2) would be ###.##
- `ENUM('y','n')` - a string object that can only be the values supplied; in this case, 'y' or 'n'


<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Unsigned Integers

An unsigned integer simply means it cannot be a negative value. 
Integer Ranges:
- `TINYINT` - 1 byte, up to 127 signed, up to 255 unsigned
- `SMALLINT` - 2 bytes, up to 32,767 signed, up to 65,535 unsigned
- `MEDIUMINT` - 3 bytes, up to 8,388,607 signed, up to 16,777,215 unsigned
- `INT` - 4 bytes, up to 2,147,483,647 signed, up to 4,294,967,295 unsigned


<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Insert Values Into Tables

```sql
INSERT INTO person
(name, dob)
VALUES('Viole Park', '1999-09-09');
```

If all fields in a table are being inserted and in the correct order, the `(name, dob)` from the example can be omitted. Otherwise, the given syntax should be used. For this example, the personID was not given because that was set to auto increment and MySQL will take care of it. 


<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Show Table Structure

```sql
DESCRIBE person;
```

This will show the structure of the 'person' table, including all fields and their data type.


<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Selecting & Displaying Information From Table

```sql
SELECT name, dob
FROM person
WHERE dob = '1999-09-09';
```
This will display a table with the headers 'name' and 'dob' and the row containing 'Viole Park' and '1999-09-09'. 

The headers can also be changed. If it is simply a case change, such as displaying "Name" instead of "name", it can be typed that way. If it needs to be a bigger change, it should use `AS` and double quotes (MySQL accepts single quotes without problem, but other database management systems (DBMS) do not, so it is good practice). 

```sql
SELECT Name, dob AS "Date of Birth"
FROM person
```
This will return all rows in the `person` table, and the headers will display as `Name` and `Date of Birth`.

Regex pattern matching can also be done using `LIKE`. A `%` indicates 0 or more while a `_` indicates exactly one.
```sql
SELECT Name
FROM person
WHERE name LIKE '%o%';
```
This will select all names that have an 'o' anywhere in the name. 

Logical operators can also be used. <, >, !=, <>, AND, OR, BETWEEN, etc.
<> is simply not equal. 
```sql
SELECT title, author
FROM book
WHERE cost BETWEEN 9.99 AND 15.99;
```

<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Delete Rows From Table
```sql
DELETE FROM person
WHERE name = 'Viole Park';
```

<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Updating & Changing Rows 
```sql
UPDATE person
SET dob = '2000-09-09'
WHERE name = 'Viole Park';
```

<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Altering a Table's Structure
```sql
ALTER TABLE person
ALTER COLUMN state CHAR(2) DEFAULT = 'CA';

ALTER TABLE person
ALTER COLUMN state DROP DEFAULT;
```

Alter is used to set or remove default values for a column. `SET` and `DROP` may be used. 
This alters the table person and makes the state field be a CHAR(2) instead of VARCHAR(20). It also sets it so that if no value is given, by default place in 'CA';

```sql
ALTER TABLE person
CHANGE COLUMN dob birthday;

ALTER TABLE person
CHANGE COLUMN name Name VARCHAR(30);
```
Change is used to rename a column or change the datatype. The first example renames dob into birthday and the second renames name into Name and changes VARCHAR(20) to VARCHAR(30).

```sql
ALTER TABLE person
MODIFY COLUMN name VARCHAR(20) NOT NULL;
```

Modify can do basically everything change can, but doesn't rename the column. `NOT NULL` means that this field requires a value. 


<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


#### Constraints - Check & Foreign Keys
 Constraints can be placed while creating a table or with alter. `CHECK` limits values.
 
 ```sql
 CREATE TABLE nurse(
 nurseID INT UNSIGNED AUTO_INCREMENT,
 name VARCHAR(30),
 age INT,
 CHECK (age >= 18)
 CONSTRAINT nurse_pk PRIMARY KEY(nurseID),
 CONSTRAINT nurse_nurseID_fk FOREIGN KEY(nurseID) REFERENCES person(personID)
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
 ```
 
 In this example, `CHECK` ensures that nurses are 18 years or older. Trying to add someone who is younger than 18 will not work. The foreign key constraint shows that the key refers to the person table, so nurseIDs must exist in personIDs.
 
 
<div align="center">■ □ ■ □ ■ □ ■ □ ■ □ ■</div>


