---
layout: post
title: Binary System 1
subtitle: SystemSoftware_02_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---
1. 이진수 체계
2. 이진수 연산의 특징
3. 이진수로 부호 있는 정수 표현하는 방법
4. binary code: 수 외의 다른 유형의 정보를 이진수 형태로 표현하는 방식

# Information representation
컴퓨터는 정보처리 장치이다. <br/>
컴퓨터 내부의 동작을 이해하기 위해서는 정보를 어떻게 표현되는지를 알아야 한다. <br/>

정보를 표현하는 방식

1. Digital form
- A set restricted to a finite number or sequence of elements/digits; thus the 
information is discrete → 이산적, 비연속적, 유한함 <br/>
ex. a digital watch, which expresses time in a numerical form using digits <br/>
현재 시각 → 십진수 두자리(수), 십진수 두자리(분), 십진수 두자리(초)<br/>
정밀하게 하기 위해서는 초를 초보다 더 세밀한 단위로 확장시키면 된다.
- Limits the precision of the information to the number of digits <br/>
정확도는 수의 자릿수에 의존한다.

2. Analog form<br/>
- A continuum is used to denote the information → 연속적 <br/>
ex. a conventional watch using hands and the angle between the hands to show the time(시간), voltages(전압), current(전류), temperature(온도) etc.<br/>
시곗바늘이 있어서 시곗바늘간의 각도를 가지고 나타낸다 <br/>
하지만 눈금이 가르키는 값이 연속적으로 표현되기 떄문에 시각을 정확히 읽어내는데는 어려움이 있다.

우리 주변에 컴퓨터가 처리하는 많은 형태의 다양한 정보들은 대부분 아날로그형태이다. <br/>
컴퓨터를 연속적이고 아날로그 형식으로 만들 수 있지만, 디지털 형식으로 만드는게 장점이 많다. <br/>
Digital systems deal with digitized information
1. 장치의 가격 저렴 → cheaper
2. 동작에 대한 신뢰도가 높아짐 → reliable
3. 그 장치가 다양한 분야에 활용될 수 있음 → greater verstility

# Why Binary?

Digital computers/systems deal with discrete elements  information, which are themselves represented physically as signals.<br/>
Signals (voltage and current) are themselves analog or continuous quantities.  <br/>
An analog to digital (A-to-D) conversion is hence required. <br/>
Digital computers only (in fact can only) manipulate numbers! <br/>

디지털 컴퓨터는 처리하고자하는 정보가 어떤 정보이든지 간에 디지털 형태로만 표현하고 처리한다. <br/>
따라서 정보가 근본적으로 아날로그 형태인 경우 (전압, 전류)에 <br/>
아날로그 형태 자체로 컴퓨터에서 처리할 수는 없고 그 정보들을 변환해야 한다. (analog to digital 변환)<br/>
디지털로 변환된 정보들은 수로 표현된다. <br/>

So what does a digital computer do?
- Receives numbers called data
- Performs operations on these numbers
- Forms new numbers
- The desired operations to be performed are also given to the computer in the form of 
numbers called instructions

디지털 컴퓨터의 동작
- 입력 전 digitize(디지털화)해서 수로 만들어서 입력을 받고
- 연산을 할떄도 수에 대한 연산	
- 연산자체도 수로 표현됨
- 최종적으로도 새 정보도 수정보

Since numbers are stored and manipulated – a number system, which is easy to represent electronically is necessary<br/>
Binary number system or a coded binary system is used. <br/>
Highly reliable electronic devices with 2 stable states are easily fabricated<br/>
Signals have 2 discrete values (hence the term binary)<br/>
binary digit – bit has 2 values 0 and 1<br/>

10진수 각 자리에 올 수 있는 수가 10가지나 돼서, <br/>
10가지의 서로 다른 상태를 표현할 수 있는 수단이 필요하다. <br/>
그러므로 컴퓨터가 수를 저장하고 조작하는데 용이한 수 체계가 이진수이다. <br/>
각 자리에 올 수 있는 수는 0, 1 두가지 밖에 없어서 2가지 상태만 표현할 수만 있으면 된다. <br/>

# Binary System
## The traditional decimal number system
Base (or radix) 10 uses ten digits (0,1,…9), each multiplied by a power of 10 
depending on its position. <br/>

__Base(기수, 숫자표기법상 기초가 되는 수)__ 가 10인 수 체계 <br/> 
십진수 각자리에 0~9까지 열 개의 숫자가 올 수 있다. <br/>
수의 위치에 따라서 우리가 베이스로 사용하는 십의 거듭제곱이 곱해진 형태로 표현 가능 (계수만 나타냄) <br/>

ex. $$7392 = 7*10^3+3*10^2+9*10^1+2*10^0$$

## The binary number system
Base 2 uses two digits (0 and 1), each multiplied by a power of 2 depending on its 
position. <br/>
ex. <br/>
$$ 1011_{2}→1*2^3+0*2^2+1*2^1+1*2^0 = 11 $$ <br/>

## Radix Point 소수점
The radix point distinguishes positive powers of 10 (or 2) from negative powers of 10 (or 2)<br/>
소수점 아래 숫자들의 10 (또는 2)의 음의 거듭제곱 형태로 표현 <br/>
$$11010.11_{2} → 1*2^4 + 1*2^3+0*2^2+1*2^1+0*2^0+1*2^{-1}+1*2^{-2} = 26.75$$
<br/>

## binary prefix
수를 이진수로 표현할 때의 단점은 <br/>
표현하고자하는 수가 조금만 커져도 해당하는 자릿수가 굉장히 많이 필요하다는 것이다. <br/>
$$ 1023_{10} (4자리) = 1111111111_{2} (10자리) $$ 
<br/>

이진수을 편하게 표기하기 위해서 __binary prefix__ 사용 <br/>

![ssw_ch2_1](https://user-images.githubusercontent.com/63347989/136072298-17be10ef-39b6-42f8-9c46-80e0e03501d6.PNG)<br/>

Computer capacity is measured in bytes, which is equal to 8 bits of information. <br/>
1 byte = 8 bits <br/>
$$ 4Kb = 2^2*210 = 2^{12} = 4096 bits $$
<br/>
$$ 4KB = 2^2*2^10*2^3 = 2^{15} = 32768 bits $$
<br/>


## Some powers of Two

![ssw_ch2_2](https://user-images.githubusercontent.com/63347989/136074306-16e703fa-8edb-4ca2-ae55-a0fa9957fc23.PNG)

# Base-r system
In general, a number expressed in base-r system <br/>
Has coefficients multiplied by power of r <br/>
$$ a_n*r^n + a_{n-1}*r^{n-1} + ... + a_2*r^2 + a_1*r^1 + a_0*r^0 + a_{-1}*r^{-1} + a_{-2}*r^{-2} + ... + a_{-m}*r^{-m} $$

Coefficients a_j can range from 0 to r-1; we enclose the coefficients in parentheses and write a subscript equal to the base ; <br/>
$$(a_na_{n-1} ⋯ a_1a_0.a_{-1}⋯ a_{-m})_r$$
<br/>
$$(4021.2)_5 → 4*5^3+0*5^2+2*5^1 +1*5^0 + 2*5^{-1} = (511.4)_{10} $$
<br/>

When the base is greater than 10, the letters of the alphabet are used to supplement the 10 
decimal digits. <br/>

![ssw_ch2_3](https://user-images.githubusercontent.com/63347989/136076067-8e77b8c8-5644-497c-9a91-5fae40516875.PNG)<br/>
대문자 소문자 구분 없다. <br/>

## Converting decimal to base-r
![ssw_ch2_4](https://user-images.githubusercontent.com/63347989/136076661-c1d22028-9551-4527-8b07-65b1e20a2d62.PNG)

소수점 같은 경우는 곱해주면 된다. (원하는 자릿수만큼만 구하고 멈춤) <br/>

![ssw_ch2_5](https://user-images.githubusercontent.com/63347989/136076966-f6fa11a8-9c6b-45f3-8483-d189a2243b32.PNG)

<br/>

![ssw_ch2_6](https://user-images.githubusercontent.com/63347989/136076959-3ff70043-2ec1-422a-817c-829f3e4dda29.PNG)

The conversion of decimal numbers with both integer and fraction part is done by converting the integer and fraction separately and then combining the two answers. <br/>
정수부와 소수부를 나눠서 계산 (정수부는 나누고 소수부는 곱하고)<br/>
최종적인 답 정수부.소수부 식으로 합치면 됨. <br/>

## Converting 
베이스가 다른 두수가 있고 둘 다 십진수가 아닌 경우에 <br/>
두 수간의 변환은 중간에 십진수 변환을 거치면 된다. <br/>
그러나 베이스간의 두 수가 거듭제곱 관계가 존재한다면 십진수를 거치지 않아도 된다. <br/>
## Converting Binary to Octal
![ssw_ch2_7](https://user-images.githubusercontent.com/63347989/136078229-8e6bf757-c1ed-4f97-8c66-95c4960d4e8f.PNG)

<br/>

## Converting binary to hexadecimal
![ssw_ch2_8](https://user-images.githubusercontent.com/63347989/136078232-fd65afbd-8b09-4897-b492-1712a7e82842.PNG)

<br/>

## Why octal & hexadecimal?
Binary use a lot of bits to represent a number.<br/>
ex. the decimal 4095 requires only 4 digits but <br/>
&nbsp; &nbsp; &nbsp; in binary – 111111111111, 12 bits/digits are needed <br/>
So, the octal & hexadecimal reduces the number of digits<br/>
$$ (4095)_{10}=(7777)_{8} (4 digits) = (FFF)_{16} (3 digits)$$
<br/>
while retain the binary system<br/> 
Simple and efficient<br/>

2진수 -> 8진수 자릿수가 1/3로 줄어듬<br/>
2진수 -> 16진수 자릿수가 1/4로 줄어듬<br/>
<br/>
컴퓨터에서 모든 표현이 2진수로 이루어져서 <br/>
8진수와 16진수가 컴퓨터 내부에서 직접적으로 사용될 순 없지만, <br/>
사람이 사용하기는 불편함이 있어서 표기, 대화할때 8진수나 16진수 이용<br/>

# Binary arithmetic
0+0 = 1, 1+0 = 0+1 = 1, 1+1 = 10, 0-0 = 0, 0-1 = 1, 1-0 = 1, 1-1 = 0 <br/>
0x0 = 0, 1x0 = 0x1 = 0, 1x1 = 1<br/>
- Addition, using carries <br/>
101101 + 100111 = 1010100 <br/>
- Subtract, using borrow <br/>
101101 - 100111 = 000110 <br/>
- Multiplication <br/>
1011 * 11 = 1011 + 1011 = 100001 <br/>