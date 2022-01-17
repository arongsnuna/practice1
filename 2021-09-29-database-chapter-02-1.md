---
layout: post
title: Relational Database 1
subtitle: DataBase_02_1
gh-repo: unhipark
gh-badge: none
tags: [database]
comments: true
---

관계형 데이터베이스는 테이블(table)의 모임으로 구성 <br/>
*Table: 행(row)과 열(column) 존재, 고유한 이름을 가지고 있음 <br/>

Example of a Table (Instructor table) <br/>

![db_ch2_1](https://user-images.githubusercontent.com/63347989/135250449-e41d9271-3cc0-432b-a3cb-a94bc0862694.png) <br/>


## Relational Model
- Table =   __Relation__
- Column  =   __Attribute__
- Row   =   __Tuple__<br/>

Example of a Relation (instructor)<br/>

![db_ch2_2](https://user-images.githubusercontent.com/63347989/135250752-5f484868-f83b-4891-8981-2781df7fb258.png)

__Sorted vs. Unsorted display of the instructor relation__

![db_ch2_3](https://user-images.githubusercontent.com/63347989/135250990-cc352406-6e93-4011-a94c-90ecbc6636a7.png)
<br/> <br/>

## Attribute Types 
1. The set of allowed values for each attribute is called the __domain__ of the attribute. <br/>
 domain: 속성이 가질 수 있든 값들의 data type<br/>
 ![db_ch2_4](https://user-images.githubusercontent.com/63347989/135252932-259cb969-8f86-48bc-a322-1ba114f8278c.png)

2. Attribute values are (normally) required to be __atomic__ (원자적); that is, indivisible. <br/>
원자적 -> 쪼개질 수 없음<br/>
![db_ch2_5](https://user-images.githubusercontent.com/63347989/135253319-1f1d3c4a-2681-4574-a17d-a348c254024e.png)

3. The special value __null__  is a member of every domain. <br/>
아래와 같은 두 가지 경우에 속성값으로 null 사용 가능
    - __Does not exist__ <br/>
    ![db_ch2_6](https://user-images.githubusercontent.com/63347989/135253910-d0aca9d3-1ab0-4dde-9854-c643017426d9.png)<br/>
     - __Unknown__<br/>
    ![db_ch2_7](https://user-images.githubusercontent.com/63347989/135254033-2add1d27-ad2f-4ee5-b665-473f30d5381a.png)<br/>


# Relation Schema and Instance
![db_ch2_8](https://user-images.githubusercontent.com/63347989/135256184-2c14c8d2-c5db-4a0b-9c97-44270052f4de.png)

Type definition ➡  Relation Schema  ➡  고정적임 <br/>
Variable ➡    Relation Instance ➡ 시간에 따라 변함<br/>

## Relation Schema
relation(table) 정의

![db_ch2_9](https://user-images.githubusercontent.com/63347989/135256420-58dd9192-260c-46e2-8f5e-8b87bf6185f2.png)

*join<br/>
![db_ch2_10](https://user-images.githubusercontent.com/63347989/135256868-3a66d536-56c3-4ff6-906f-deadf1cd2c73.png)<br/>
두개의 table을 결합해서 사용자가 원하는 정보를 찾을 때 join 사용. <br/>
한 table 안에 여러개의 속성이 있을 때 속성 값은 중복이 될 수 없음.

? Waston builing에 있는 교수들에 대한 모든 정보? <br/>
department relation 만 주어졌을 때는 알 수 없지만, instructor relation과 join하면 위 질문에 대한 답을 찾을 수 있음.

## Relation Instance
A snapshot  of the data in the database at a given instant in time. <br/>
DB 안에 저장된 data <br/>
![db_ch2_11](https://user-images.githubusercontent.com/63347989/135257668-bd6cbafe-7643-4956-b0de-c1dc4d5f9509.png)

# DATABASE
A database consists of multiple relations.

- GOOD DESINGN<br/>
![db_ch2_12](https://user-images.githubusercontent.com/63347989/135264831-2776c57e-f66c-479b-a79c-56728df0661b.png)

- BAD DESIGN<br/>
    -  repetition of information (e.g., two students have the same instructor)<BR/>
    ![db_ch2_14](https://user-images.githubusercontent.com/63347989/135264881-47c2e564-7309-4b7c-a3c2-09c64d0e1b07.png)<BR/>
    - the need for null values  (e.g., represent an student with no advisor) <BR/>
    ![db_ch2_13](https://user-images.githubusercontent.com/63347989/135264883-e1775c25-9447-40e3-b40d-d338f8032def.png)

# KEYS
하나의 tuple을 다른 tuple들로 부터 구별하는 방법 (attributes 중에서 고유한 값을 가지는 것)<br/>

|attribute|key|
|---:|---:|
|Student number|O|
|SSN (Social security number) |O|
|Email address |O|
|Student name |X|
|{Student number, Student name} |O|
<br/>
1. Superkey<br/>
A set of attributes that allow us to identify uniquely a tuple in the relation.<br/>

    |attribute | superkey|
    |---:|---:|
    |{ID} |O|
    |{SSN} |O|
    |{ID, name}|O|
    |{ID, SSN} |O|
    |{SSN, name, dept_name} |O|
    |{name}|X|
    |{name, dept_name}|X|

2. Candidate key<br/>
A __minimal superkey__ for a relation. <br/>
A superkey such that __no proper subset is a super key__. <br/>
*proper subset = 부분집합들 - 공집합 - 자기자신<br/>
super key가 candidate key를 포함함. <br/><br/>
_instructor (ID,  name, dept_name, salary, SSN)_<br/>

    |attribute | superkey|candidate key|
    |---:|---:|---:|
    |{ID} |   O  | O|
    |{SSN} |O| O|
    |{ID,   name} | O|X|
    |{ID, SSN}  |O|X|
    |{SSN, name, dept_name} | O|X| 
3. <u>Primary Key</u> <br/>
A candidate key that is chose by the database designer. <br/>
candidate key는 primary key를 포함. <br/>
primary key는 밑줄를 그음.

4. Foreign Key <br/>
A relation may include among its attributes the primary key of another relation.<br/>
한 relation의 pk가 다른 relation의 pk일 때, foreign key 라고 한다.<br/>
![db_ch2_15](https://user-images.githubusercontent.com/63347989/135272087-65d047e7-70e8-45dd-884d-74946884d830.png)
<br/>

**참고**<br/>
_classroom(building, room_number)_<br/>

|building|room_number|
|---:|---:|
|G|211|
|G|210|
|H|211|
|H|210|

<br/>

|attribute | superkey|candidate key|
|---:|---:|---:|
|{building, room_number}|O|O|
|{building}|X||
|{room_number}|X||

**<br/>



## Schema Diagram for University Database
![db_ch2_16](https://user-images.githubusercontent.com/63347989/135272557-3732685d-e152-4dba-9b7d-63f8798fdd04.png)
- Relation
- Attribute
- Primary Key
- Foreign Key




