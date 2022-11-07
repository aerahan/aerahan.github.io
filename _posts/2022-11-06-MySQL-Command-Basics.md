---
title: "MySQL Command Basics"
layout: post
date: 2022-11-06-22
feature_image: images/
tags: [mysql, database, basics]
---

<!--more-->

Most MySQL commands are self explanatory, so I'll just generally drop an example or syntax with a short explanation. 

<div align="center">■□■□■□■□■□■</div>

#### Create a Database
To create a database and change to that database:

```sql
-- this is a comment. Ensure there is a space after the two hyphens. 
CREATE DATABASE staff;
USE staff;
```

#### Create a Table within a Database

```sql
CREATE TABLE person(
personID INT UNSIGNED AUTO_INCREMENT,
name VARCHAR(20) NOT NULL,
address VARCHAR(20),
zipCode CHAR(5),
dob DATE,
CONSTRAINT person_pk PRIMARY KEY(personID)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

Constraints can be added using `CONSTRAINT`, including a primary key/unique identifier. In this example, `personID` became the primary key. 


#### Data Types

Some data types that MySQL accepts include:
- `VARCHAR(20)` - a variable length string; in this case, can contain up to 20 letters
- `CHAR(5)` - a fixed length character/string; in this case, 5
- `INT` - an integer value
- `DATE` - a date, stored as `'yyyy-mm-dd'`
- `DECIMAL(X,Y)` - an integer value of X length with Y digits after the point. (5,2) would be ###.##
- `ENUM('y','n')` - a string object that can only be the values supplied; in this case, 'y' or 'n'


#### Unsigned Integers

