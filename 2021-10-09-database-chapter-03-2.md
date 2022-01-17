---
layout: post
title: Introduction to SQL 2
subtitle: DataBase_03_2
gh-repo: unhipark
gh-badge: none
tags: [database]
comments: true
---

# 2. Basic Query Structure
A typical SQL query has the form:
```
 select   A1, A2, ..., An
 from     r1, r2, ..., rm
 where    P;
```
Ai represents an attribute <br/>
Ri represents a relation<br/>
P is a predicate(조건) <br/>

The result of an SQL query is a __relation__.

## The select Clause
The __select clause__ list the attributes desired in the result of a query<br/>
corresponds to the __projection operation__ of the relational algebra<br/>

    Π (ID, salary) (instructor)
    → select ID, salary from instructor
![db_ch3_11](https://user-images.githubusercontent.com/63347989/136610135-90d816fe-165f-445f-8e79-640969778d9f.png)

### DISTINCT
SQL allows duplicates in relations as well as in query results.<br/>
To force the __elimination of duplicates__, insert the keyword __distinct__ after select <br/>

```
select  distinct  dept_name
from    instructor
```
### ALL
The keyword all specifies that __duplicates not be removed__.
```
select  all dept_name
from    instructor
```

```
select  dept_name
from    instructor
```

### *
An asterisk in the select clause denotes __“all attributes”__
```
select *
from   instructor
```
### arithmetic expressions 
The select clause can contain arithmetic expressions involving the operation, +, –, *, and /, and operating on constants or attributes of tuples.

```
select ID, name, salary/12
from   instructor
```
## The where Clause
The __where clause__ specifies conditions that the result must satisfy <br/>
Corresponds to the selection __predicate__ of the relational algebra<br/>

    σ salary>85000 (instructor)
    → select * from instructor where salary > 85000
![db_ch3_12](https://user-images.githubusercontent.com/63347989/136611199-5a9a6525-2c32-4ec3-b288-0696583e826d.png)<br/>

ex. To find all instructors in ‘Comp. Sci.’ department with salary > 70000
```
select    name
from      instructor
where     dept_name = ‘Comp. Sci.'  and  salary > 70000
```
## The from Clause
The __from clause__ lists the relations involved in the query<br/>
Corresponds to the Cartesian product operation of the relational algebra<br/>

    instructor X teaches (Cartesian product)
    → select from instructor, teaches

generates every possible instructor – teaches pair, with all attributes from both relations<br/>

Cartesian product not very useful directly, but useful combined with where-clause condition (selection operation in relational algebra). <br/>

### JOIN Query

```
select ...
from R1, R2
where ...
```
ex.
```
select     name, course_id
from       instructor, teaches
where      instructor.ID = teaches.ID
```

__Step 1.__
Generate a Cartesian product of the relations listed in the from clause
```
select  *
from    instructro, teaches
```
__Step 2.__
Apply the predicated specified in the where clause on the result of Step 1
```
select  *
from    instructor, teaches
where   instructor.ID = teaches.ID
```
__Step 3.__ For each tuple in the result of Step 2, output the attributes specified in the select clause
```
select   name, course_id
from     instructor, teaches
where    instructor.ID = teaches.ID
```
→ “For all instructors who have taught courses, find their names and the course ID of the courses they taught.”<br/>
<br/> 
Π (name, course_id)(σ (instructor.ID = teaches.ID)(instructor X teaches))

### NATURAL JOIN 
Natural join matches tuples with the same values for all common attributes, and retains only one copy of each common column

    instructor  ∞  teaches

    → select * from instructor natural join teaches

### JOIN and NATURAL JOIN
NATURAL JOIN O→O JOIN (cartesian product) <br/>
NATURAL JOIN X←X JOIN (cartesian product) <br/>
