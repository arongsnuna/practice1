---
layout: post
title: Introduction of Database
subtitle: DataBase_01
gh-repo: unhipark
gh-badge: none
tags: [database]
comments: true
---

# DATABASE VS. DATABASE MANAGEMENT SYSTEMS
 A __database__ is an organized collection of data.<br/>
 __Database management systems (DBMSs)__ are specially designed software applications that interact with the user, other applications, and the database inself to capture and analyze data.<br/>
 > USER <-> 응용프로그램 <-> DBMS <-> DB <BR/>
<br/>
![db_ch1_1](https://user-images.githubusercontent.com/63347989/135235643-87b45896-2985-4156-aead-e981f976762a.png)
<br/>
<br/>

ex 1. e-campus <br/>
USER <-> E-CAMPUS <-> DBMS <-> DB <br/>

ex 2. 은행<BR/>
ATM <-> DBMS <-> DB<BR/><BR/>

DB랑 DBMS는 같은 서버에 있기도 하고 다른 서버에 있기도 하다.<BR/>
DB는 disk(영구 저장) 또는 memory(임시 저장)에 저장된다. <br/>
DB가 매우 클 수도 있기 때문에, DBMS는 편리(convenient)하고 효율적(efficient)이어야 한다. <BR/>
<BR/>

# Database System Applications

- Banking: transactions
- Airlines: reservations, schedules
- Universities:  registration, grades
- Sales: customers, products, purchases
- Online retailers: order tracking, customized recommendations
- Human resources:  employee records, salaries, tax deductions

=> Databases touch all aspects of our lives



# Drawbacks of using file systems to store data
In the early days, database applications were built directly on top of file systems.<BR/>
USER <-> 응용프로그램 <-> DATA (file system)<br/>
즉, 응용프로그램이 데이터 관리를 해줘야하는데, 그렇게 되면 응용프로그램 개발이 어려워진다. <BR/>

1. Data redundancy (중복성) and inconsistency (불일치) <BR/>
   __Duplication__ of information in different files lead to data __inconsistency__. <br/>
   
   ![db_ch1_2](https://user-images.githubusercontent.com/63347989/135237602-fa3233fe-6f71-4b66-ac7e-7a9af40e4c45.png)


1. Difficulty in accessing data <br/>
Need to write a new program to carry out each new task.
<!--![db_ch1_3](https://user-images.githubusercontent.com/63347989/135237897-a21c1b38-ad8d-40be-952a-37a9749173ed.png)-->
3. Integrity problems <br/>
   Developers should add appropriate code in the programs <br/>
   and it's hard to add new constraints or change existing ones

4. Atomicity problems (원자성 문제) <br/>
   If a failure occurs, the data be restored to the consistent state that existed prior to the failure. <br/>
   프로그램은 순서대로 진행되는데, 중간에 오류 발생시 원상태로 복구되어야 한다. <br/>
   DBMS는 자동적으로 원상대로 복구되지만 파일 시스템 사용시 그렇지 못하다. 
5. Concurrent access by multiple users <br/>
   Some performance needs Concurrent access, but Uncontrolled concurrent accesses can lead to inconsistencies. <br/>

   <img src="https://user-images.githubusercontent.com/63347989/135243279-21e78556-9532-4c5c-b47a-7ef61072fcc7.jpg" width="600" height="200">

6. Security problems <br/>
   Hard to provide user access to some, but not all, data. <br/>
   민감한 data가 있을 수도 있으니 DBMS는 접근 권한 부여를 한다.