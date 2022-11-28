---
title: "MySQL - Functions"
layout: post
date: 2022-11-23
feature_image: images/
tags: [mysql, database]
---




<!-- more -->

Read about MySQL [Command Basics](https://aerahan.github.io/MySQL-Command-Basics).
Read about MySQL [Query Operations](https://aerahan.github.io/mysql-query-operations).

This is the third installment!


Single Row Functions 
LOWER(), UPPER()
LENGTH()
INSTR()
SUBSTR()
CONCAT()

Numeric
ROUND()
TRUNCATE()
MOD()

Multi Row Functions
not func but multi table join
COUNT()
SUM()
MIN()
MAX()

More SELECT clauses...
GROUP BY
HAVING
ORDER BY


IMAGE

Let's find the names of recipes that are desserts.

```sql
SELECT name
FROM recipe
WHERE 

```

Multi-table Joins


LOWER(string) will take the string in the parenthesis and return a string where it is all lowercase. UPPER(string) will do the exact opposite. MySQL itself isn't case sensitive, but these functions exist.

Example:
```sql
SELECT title
FROM song
WHERE LOWER(title) = 'Stay With Me';
```

LENGTH(string) will take a string and return the length of it (number of characters in the string). 

Example:
```sql
SELECT title
FROM recipe
WHERE LENGTH(title) > 10;
```

INSTR(string1, string2) will look at string1 and see if string2 is within it - if it isn't, it will return 0. If it is, it will return the starting position. Index starts at 1. 

SUBSTR(string, n, m) will take a string, start at position n for up to m characters, and return the new string. 

INSTR() can often be used with SUBSTR(). 

Example:

Say you have different chocolate recipes such as "Chewy Chocolate Chip Cookies" and "Creamy Fudgy Chocolate Cake". 
```sql
SELECT title, SUBSTR(title, INSTR(title, "chocolate"), LENGTH(title)-INSTR(title, "chocolate")) AS "Chocolate Types"
FROM recipe
WHERE title LIKE "%chocolate%";
```
This query would return "Chocolate Chip Cookies" and "Chocolate Cake" in the 2nd column. 


CONCAT(string1, string2) will add string2 to string1. 

Example:
```sql
SELECT CONCAT(recipe.title, ' from ', source.firstName) AS "Recipe and Source"
FROM recipe
LEFT JOIN source ON source.sourceID = recipe.sourceID;
```


ROUND(n, m) will round the number n to m decimal places. 
