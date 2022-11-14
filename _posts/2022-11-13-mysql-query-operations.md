---
title: "MySQL - Query Operations"
layout: post
date: 2022-11-13
feature_image: images/mysql_query_operations.png
tags: [mysql, database]
---

Following my post on [MySQL Command Basics](https://aerahan.github.io/MySQL-Command-Basics), this post goes more in-depth on querying data and the operations used. 

<!--more-->

### Relation Characteristics
- all cells in a relation must hold a single value
- no repeating groups (more than one attribute from the same physical & logical domain, such as author1 and author2)
- all values for an attribute/column must be part of the same logical and physical domain
- tuples/rows must be unique and have a primary key 
- the order of tuples and attributes (rows and columns) must be unimportant 
- each attribute within a relation must have a unique name

In the example of `EMPLOYEE(ssn, name, dob)`, `ssn`, `name`, and `dob` are attributes. Tuples would be instances of the EMPLOYEE relation, such as `123-45-6789, Aera, 01/01/2001`. 


<div align="center">────── ⋅ ⋅ ──── ✩ ──── ⋅ ⋅ ──────</div>


### Relational Algebra
Relational algebra is a way to query data in a relation. It is similar to sets and set operations. It consists of variables that represent relations and operators that represent the actions taken with the relations. The `DISTINCT` keyword can be used to remove duplicate rows from a result set, such as `SELECT DISTINCT...`

There are three basic types of operators:
1. Operations to remove parts of relation (Selection - only tuples that meet criteria, Projection - only specified attributes appear)
2. Set based operations (Union, Intersection, Difference)
3. Operations to join tuples in two relations (Product, Joins)


<div align="center">────── ⋅ ⋅ ──── ✩ ──── ⋅ ⋅ ──────</div>


#### Projection
A projection of a relation produces a new relation, containing selected columns from the original relation. This ends up limiting the attributes that appear so that only the selected attributes will appear. While calling a projection, the order of columns in the resulting relation can be rearranged for display. Duplicate tuples/rows will be removed. 

Syntax: `RELATION[attr1, attr2, attr4]` and `π attr1,attr2,attr4 (RELATION)` (where after pi, the attributes are a subscript)

Example:
STUDENT

| stuID      | name       | major      |
| ---------- | ---------- | ---------- |
| 100101     | Aera Han   | CyberSec   |
| 100102     | June Park  | Engineerng |
| 100103     | AJ Wiliams | Photogrphy |

`STUDENT[name]` would display 

| name       |
| ---------- |
| Aera Han   |
| June Park  |
| AJ Wiliams |


In MySQL, projection can be done through the `SELECT` clause and selection can be done through the `WHERE` clause of a `SELECT` statement. 

Example:
```sql
SELECT name
FROM student;
```


<div align="center">── ⋅ ⋅ ── ✩ ── ⋅ ⋅ ──</div>


#### Selection
Selection is used to get a result set if specific conditions are met. Conditionals include =, >, >=, <, <=, ¬, &and;, &or;

Syntax: `RELATION WHERE condition`. 

Example:
`STUDENT WHERE major = 'CyberSec' OR major = 'Engineering'`

| stuID      | name       | major      |
| ---------- | ---------- | ---------- |
| 100101     | Aera Han   | CyberSec   |
| 100102     | June Park  | Engineerng |


<div align="center">── ⋅ ⋅ ── ✩ ── ⋅ ⋅ ──</div>

In MySQL, selection can be done through the `WHERE` clause of a `SELECT` statement. 

Example:
```sql
SELECT name
FROM student
WHERE major = 'CyberSec' OR major = 'Engineering';
```

#### Unions
A union of two relations will produce a relation with all the tuples in each relation. The two relations must be union compatible, meaning both relations have the same number of attributes that are in the same order (meaning corresponding attributes should come from the same physical and logical domain). The resulting relation will be a relation and must meet relation characteristics. 

Syntax: `A ∪ B`, `A + B`, `A UNION B`

Example: 
You have two relations of students - one, MS_STUDENT for middle schoolers, and another, HS_STUDENT for high schoolers.
```sql
MS_STUDENT(stuID, lastName, firstName, grade)
HS_STUDENT(id, ln, fn, grade)
```
These could be successfully unioned.

```sql
SELECT lastName
FROM ms_student
UNION
SELECT ln
FROM hs_student;
```

The union operation is commutative, meaning that `A ∪ B` = `B ∪ A`. The order that the two relations are supplied will result not change the result. 


<div align="center">── ⋅ ⋅ ── ✩ ── ⋅ ⋅ ──</div>


#### Difference
The result of a difference is all tuples from the first given relation that do not exist in the second given relation. The relations, like a union, must be union compatible. A difference, however, is NOT commutative, and the order matters. `A - B` does NOT equal `B - A`. 

Syntax:
`A-B`
`A DIFFERENCE B`

Note: MySQL does not enforce union compatibility for differences (it does for unions). In order to make sure it is union compatible, it must be enforced in the statement. 

Subqueries can be used in MySQL to perform a difference operation. See more on [Subqueries](#subqueries) below. 

Example: 

To select all students that aren't part timers:

```sql
SELECT * 
FROM student
WHERE stuID NOT IN(
  SELECT stuID
  FROM part_timers);
```


<div align="center">── ⋅ ⋅ ── ✩ ── ⋅ ⋅ ──</div>


#### Intersection
An intersection is a relation that contains the tuples that exist in BOTH relations supplied. In a Venn diagram, this would be the middle section of two intersecting sircles. Relations must be union compatible. Like unions, intersections are commutative. This means that `A INTERSECT B` is the same as `B INTERSECT A`. 

Syntax: `A ∩ B` and `A INTERSECT B`

Intersections can use `IN` and `EXISTS` with subqueries. 

STUDENT

| stuID      | name       | majorCode  |
| ---------- | ---------- | ---------- |
| 100101     | Aera Han   | CYS        |
| 100102     | June Park  | ENR        |
| 100103     | AJ Wiliams | PHG        |


PART_TIMERS

| stuID      | job        | 
| ---------- | ---------- | 
| 100101     | tutor      | 
| 100103     | assistant  | 


```sql
# display all students with a job
SELECT student.stuID, student.name
FROM student
WHERE stuID IN (
  SELECT stuID
  FROM part_timers);
  
# do the same with EXISTS
SELECT student.stuID, student.name
FROM student
WHERE EXISTS(
  SELECT *
  FROM part_timers
  WHERE part_timers.stuID = student.stuID);
```

If the tables or statements aren't union compatible, an intersection cannot be used. Instead, a `JOIN` can be used. 
```sql
FROM student INNER JOIN part_timers
ON student.stuID = part_timers.stuID;
```
See more on [Joins](#joins) below. 


<div align="center">── ⋅ ⋅ ── ✩ ── ⋅ ⋅ ──</div>


#### Product
A product gives a set of tuples from combining each tuple in one relation with each tuple in another relation. It's also called a *Cartesian product* and a *Cross product*. 

Syntax: `A x B` and `A PRODUCT B`. It is typically used with other operations such as JOIN. 

With a relation that has 5 rows and another relation with 5 rows, the resulting relation would display 25 rows. 


<div align="center">── ⋅ ⋅ ── ✩ ── ⋅ ⋅ ──</div>


#### Joins
Types of joins: 
- Equijoin
- Natural join
- Outer joins (directional joins)

`EQUIJOIN` and `NATURAL JOIN` select tuples where the 'ON' attributres have non-null values in both relations (intersecting only, center of Venn diagram). 

`LEFT OUTER JOIN` selects tuples where the 'ON' attributres have non-null values in the left side (of a Venn diagram), which is the first relation.

`RIGHT OUTER JOIN` selects tuples where the 'ON' attributes have non-null values in the right side, which is the second relation.

`FULL OUTER JOIN` would select tuples where the 'ON' attribute has non-null values in either side, meaning the whole Venn diagram. 


For equijoin, the specified attributes are matched with an equality test. An equijoin is called with JOIN. 

A natural join is similar to an equijoin but only one of the joint attributes is in the final result. 

For example, equijoining STUDENT and MAJORS would display two columns of majorCode, but natural joniing them would only show one. 


<div align="center">────── ⋅ ⋅ ──── ✩ ──── ⋅ ⋅ ──────</div>


### Subqueries
Subqueries allow for two queries to be combined. 

Example: 
STUDENT

| stuID      | name       | majorCode  |
| ---------- | ---------- | ---------- |
| 100101     | Aera Han   | CYS        |
| 100102     | June Park  | ENR        |
| 100103     | AJ Wiliams | PHG        |

MAJORS

| majorCode  | major      | 
| ---------- | ---------- | 
| CYS        | Cybrsecrty | 
| ENR        | Engineerng | 
| PHG        | Photogrphy |


```sql
SELECT majorCode
FROM student
WHERE name = 'Aera Han';

SELECT major
FROM majors
WHERE majorCode = 'CYS'; # result of previous query
```

The first query becomes a subquery. Here's the modified query:
```sql
SELECT major
FROM majors
WHERE majorCode IN (
  SELECT majorCode
  FROM student
  WHERE name = 'Aera Han');
```

Using `=` will handle only one result from the subquery while using `IN` will handle any number of results. 

Subqueries should be in parenthesis. There can be many nested. 


<div align="center">────── ⋅ ⋅ ──── ✩ ──── ⋅ ⋅ ──────</div>

