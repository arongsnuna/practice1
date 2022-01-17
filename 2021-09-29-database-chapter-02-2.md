---
layout: post
title: Relational Database 2
subtitle: DataBase_02_2
gh-repo: unhipark
gh-badge: none
tags: [database]
comments: true
---

# Relational Algebra (관계대수)
*Λ -> AND<br/>
*V -> OR


Relation에 수행하는 연산의 집합
- Selection (선택 연산)
- Projection (추출 연산)
- Natural Join (자연 조인)
- Cartesian product (카티션 곱)
- Union (합집합)
- Intersection (교집합)
- Set difference (차집합)<br/>

--------------

<br/>

|||INPUT|OUTPUT|
|---:|---:|---:|---:|
|SELECTION|σ| relation|relation|
|PROJECTION|Π|relation|relation|
|NATURAL JOIN|▷◁|relation 2개|relation 1개|
|CARTESION PRODUCT |X|relation 2개|relation 1개|
|UNION|U|relation 2개|relation 1개|
|INTERSECTION|^|relation 2개|relation 1개|
|SET DIFFERENCE|-|relation 2개|relation 1개|

<br/>

-------------
-------------




1. Selection (선택 연산) - σ <br/>
선택 조건을 만족하는 Relation의 tuples을 출력한다.<br/>
__σ condition(relation)__ <br/>
ex. σ salary>$85,000 (instructor) <br/>
ex. σ salary>$85,000 Λ dept_name=“Physics”(instructor)<br/>
![db_ch2_17](https://user-images.githubusercontent.com/63347989/135275153-d891fbb6-e2e2-4688-a0ac-4a2f3ce6dde2.png)

2. Projection (추출 연산) - Π <br/>
Relation에서 선택된 attributes를 출력한다. <br/>
__Π attribute-list(relation)__<br/>
ex. Π ID, salary(instructor) <br/>
![db_ch2_18](https://user-images.githubusercontent.com/63347989/135275150-7987ef60-00ab-4922-804e-8f07a072db1d.png)


3. Natural Join (자연 조인) -  ▷◁ <br/>
같은 이름을 가지고 있는 attributes에서 두 relations이 같은 값을 가지고 있는 tuples의 쌍을 출력한다.  <br/>
__relation1 ▷◁ relation2__<br/>
![db_ch2_19](https://user-images.githubusercontent.com/63347989/135276519-166fab19-6302-4925-a506-9e7832c32e68.png)<br/>
*((r1 ∞ r2) ∞ r3) ∞ r4 = (r1 ∞ r2) ∞ (r3 ∞ r4)<br/>

4. Cartesian product (카티션 곱) - x <br/>
두 relations부터 가능한 모든 tuple의 쌍을 출력한다.<br/>
__relation1 x relation2__<br/>
![db_ch2_20](https://user-images.githubusercontent.com/63347989/135279233-ac66a328-15fd-4f13-8bbf-15dd0b59b933.png) <br/>
((r1 X r2) X r3) X r4 = (r1 X r2) X (r3 X r4) <br/>


5. Union (합집합) - U <br/>
__relation1 U relation2__<br/>
![db_ch2_22](https://user-images.githubusercontent.com/63347989/135281514-7cc6a711-e441-433a-a502-ed18a76574ba.png)

6. Intersection (교집합) - ^<br/>
__relation1 ^ relation2__<br/>
![db_ch2_23](https://user-images.githubusercontent.com/63347989/135281519-955971d2-373a-4553-b9f6-25bb87c4afd0.png)

7. Set difference (차집합) <br/>
__relation1 - relation2__<br/>
![db_ch2_24](https://user-images.githubusercontent.com/63347989/135281523-b88117c0-5caf-4db0-addb-e61502ffee84.png)
<br/>

------------

__참고1__ <br/>

Exercise <br/>

Give an expression in the relational algebra to express each of the following queries <br/>

(1) Find the name of all instructors in the Physics department<br/>

__Π name(σ dept_name='physics'(Instructor))__ <br/>

(2) Find the name of all instructors in the Physics department whose salary is greater than $60,000 <br/>

__Π name( σ (dept_nmae='physics')Λ(salary>60000)(instructor))__<br/>

__= Π name( σ salary>60000( Π name, salary( σ dept_name='physics'(instructor))))__


----------


__참고2__ <br/>

![db_ch2_25](https://user-images.githubusercontent.com/63347989/135283855-8a212a58-cb1c-41dc-a819-b7c7239c2c33.png) <br/>

---------------------

__참고3__<BR/>

![db_ch2_21](https://user-images.githubusercontent.com/63347989/135279880-a6201a92-f694-427b-af50-d55965049f49.png) <br/>

R ∞ S = R X S<br/>

---------------
__참고4__<br/>

![db_ch2_26](https://user-images.githubusercontent.com/63347989/135290760-0ebc97e9-7e39-47a1-9a83-48d3cfc7d574.png) <br/>
-------------
<br/>



---------------
__참고5__<BR/>

ex. <br/>

![db_ch2_27](https://user-images.githubusercontent.com/63347989/135285116-91de09b0-5180-4259-81d7-9bf09b551688.png)<br/>

![db_ch2_28](https://user-images.githubusercontent.com/63347989/135285123-1d448b42-9ad4-45b2-9259-2bd00af62eec.png)<br/>

![db_ch2_29](https://user-images.githubusercontent.com/63347989/135285124-97a53558-eb2d-4737-bdfb-7557c5c4992c.png)<br/>

-----


