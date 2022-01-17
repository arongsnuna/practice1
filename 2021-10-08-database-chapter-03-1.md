---
layout: post
title: Introduction to SQL 1
subtitle: DataBase_03_1
gh-repo: unhipark
gh-badge: none
tags: [database]
comments: true
---
# 1. Data Definition
Data-definition laguage (DDL)<br/>
- Defining relation schema 정의
- Deleting relation 삭제
- Modifying relation schema 변경

## Basic Types in SQL 
- __char(n).__ : Fixed length (고정된 길이) character string, with user-specified length n.
- __varchar(n).__ : Variable length (가변길이) character strings, with user-specified maximum length n.
- __int.__  Integer (a finite subset of the integers that is machine-dependent). 정수
- __smallint.__  Small integer (a machine-dependent subset of the integer domain type). 작은정수
- __numeric(d,p).__  Fixed point number, with user-specified precision of p digits, with d digits. (총 d 자리 숫자, 소수점 이하 p 자리) 실수
    - e.x) numeric (3,1) = xx.x
- __real, double precision.__  Floating point and double-precision floating point numbers, with machine-dependent precision. 실수

### char vs. vchar <br/>
- char - fixed length<br/>
![db_ch3_1](https://user-images.githubusercontent.com/63347989/136596825-74d8b1c1-9784-46f0-906c-df6bf62fdf73.png) <br/>
빈 공간은 비어있는 공간으로 disk 상에 냅둔다. <br/>
- vchar - variable length<br/>
![db_ch3_2](https://user-images.githubusercontent.com/63347989/136596822-25062c4f-eeda-4719-b995-827242fba3a4.png)<br/>
max로 10개의 문자 저장 가능하다. <br/>
그 중 필요만큼만 저장하기 위한 disk 공간만 할당하고 나머지 공간은 할당하지 않는다. <br/>

## Create Tabel Construct 
 
An SQL relation is defined using the __create table__ command <br/>

![db_ch3_3](https://user-images.githubusercontent.com/63347989/136597536-948c0bd2-a522-430e-8280-d56dfd16c996.png) <br/>

r is __the name of the relation__<br/>
each Ai is __an attribute name__ in the schema of relation r<br/>
Di is __the data type__ of values in the domain of attribute Ai<br/>

ex. ![db_ch3_4](https://user-images.githubusercontent.com/63347989/136597828-35bd2d4a-65df-4d9d-bea4-7e64a50804ce.png)

### __Integrity constraints__

![db_ch3_5](https://user-images.githubusercontent.com/63347989/136601982-e701f9ed-9a5f-4289-b4fa-72a24370564a.png)


1. not null<br/>
null value is not allowed for that attribute <br/>
ex1. insert into instructor values (‘10211’, ’Smith’, ’Biology’, 66000);<br/>
ex2. insert into instructor values (‘10212’, null, ’Biology’, 66000); → ERROR! <br/>
ex3. insert into instructor values (‘10213’, ‘Smith’, ’Biology’, null);<br/>

2. primary key (A1, ..., An)<br/>
__primary key__ declaration on an attribute ensures to be __not null and unique__ <br/>
ex1. insert into instructor values (‘10211’, ’Smith’, ’Biology’, 66000);<br/>
ex2. insert into instructor values (null, ‘Smith’, ’Biology’, 66000); → ERROR! <br/>
ex3. insert into instructor values (‘10211’, ‘Kim’, ’Physics’, 91000); → ERROR! <br/>
    ``` 
    create table intructor (
        ID char(5) PRIMARY KEY, 
        ...
    );
    ```
    위와 같이 해도 된다. <br/>
    그러나, PRIMARY KEY가 두개 이상일 때는 위와 같이 코드를 적으면 틀린 표현이다. <br/>

3. foreign key (Am, ..., An) references r<br/>
![db_ch3_6](https://user-images.githubusercontent.com/63347989/136603407-4782de11-4a6f-4ac8-bb71-4def4d0f19a3.png)<br/>
![db_ch3_7](https://user-images.githubusercontent.com/63347989/136605088-dfa71e2c-0171-4ff8-b69c-a69bc23b11c1.png)<br/>
FK 값은 반드시 참조하고 있는 PK에 값이 존재 해야한다.<br/>
    - 만약 department relation에 industrial engineering 없을때, <br/>
    insert into instructor 
      values (‘99999’, ’JW KIM’, ’Industrial Engineering’, 65000); → ERROR! <br/>
    - intructor table(fk)을 department table(pk) 보다 먼저 만들면 <br/>
    ERROR: Cannot add foreign key constraint<br/>

## Insert Statement
![db_ch3_8](https://user-images.githubusercontent.com/63347989/136606307-fa83913e-b6b4-414f-af54-e98d6ba257ab.png)<br/>
속성이 문자열인 경우, ' '해준다. 

## Alter Table
기존 테이블 변경

- alter table r add A  D (추가)<br/>
where A is the name of the attribute to be added to relation r  and D is the domain(datatype) of A.
All tuples in the relation are assigned __null__ as the value for the new attribute.  <br/>
ex. alter table instructor add monthly_salary numeric (8,2) 
- alter table r drop A (삭제)<br/>    
where A is the name of an attribute of relation r<br/>
ex. alter table instructor drop salary

## Drop Table 기존 테이블 삭제 
- drop table r (create, alter, drop) <br/>
Delete all tuples of and the schema for r<br/>
![db_ch3_10](https://user-images.githubusercontent.com/63347989/136609025-4f9cdae9-de8b-4ae6-9bb4-09b429c84bcb.png)
- delete from r (insert, delete, update) <br/>
Delete all tuples of r, bur __retain relation r__<br/>
![db_ch3_9](https://user-images.githubusercontent.com/63347989/136609027-fd34b4cb-02ca-4bae-bb53-ee886cd0a9a7.png)

