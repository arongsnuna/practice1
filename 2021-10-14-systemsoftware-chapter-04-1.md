---
layout: post
title: Introduction to Assembly Programming 1
subtitle: SystemSoftware_04_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Assembly language 
고급언어는 실행하기 위해서 컴파일이 필요함.<br/>
컴파일러가 고급언어 프로그램을 목적 프로그램 (object program) 변환 혹은 번역하는 과정에서 <br/>
컴파일러가 만들어내는 1차 출력물은 어셈블리 프로그램임. <br/>
어셈블리 프로그램은 대부분의 경우, 프로그래머가 직접 작성하는 것이 아닌, 컴파일러가 생산해내는 중간 출력물임. <br/>
그럼에도 불구하고 프로그래머가 직접 작성해야하는 경우도 있음. <br/>
- Reasons for assembly programming 
    - To improve performance 성능개선<br/>
    특정 기능을 하는 함수를 고급언어로 만든 후, 컴파일러가 어셈블리 프로그램 만들어낸 경우와<br/>
    똑같은 기능을 하는 함수를 프로그래머가 직접 어셈블리 프로그램으로 만들어낸 경우를 비교해보면 <br/>
    대부분의 경우는 사람이 직접 어셈블리어 함수를 만들어낸 경우가 크기가 더 작음. 즉 더 효율적임.<br/>
    - There are sections of code which must be written in assembly language <br/>
    고급언어로는 프로그래밍 할 수 없고 반드시 어셈블리 언어로 작성되어야하는 부분이 있음.<br/>
    특히 하드웨어 직접 접근하는 코드는 고급언어가 제공하는 문법으로 표현할 수 없는 경우도 있음.<br/>
    운영체제와 같이 그 자체가 하드웨어와 밀접하게 연관되어서 동작하는 경우에는 이런 케이스가 있음.
    - __To learn how a particular CPU works__ <br/>
    특정한 CPU의 동작을 깊게 이해할 수 있음. <br/>
    어셈블리 프로그래밍은 CPU architecture에 대한 이해가 필수적임. <br/>
- Assembly language is 
    - Machine dependent <br/>
    CPU 마다 다름.
    - But it follows universal format<br/>
    공통적인 포맷같은거는 존재함.

# Machine language vs. Assembly language 
덧셈 연산을 할 수 있는 machine instruction이 하나 있음. <br/>
To add EAX and EBX registers together and store the result back into EAX <br/>
EAX register에 담긴 32비트 수와 EBX register에 담긴 32비트 수를  더해서 결과값을 EAX register에 저장하도록 하는 machine instruction(2진수 형태)
- Machine language: numerical-coded instruction <br/>
2바이트로 표현됨. <br/>
![ssw_ch4_1](https://user-images.githubusercontent.com/63347989/137321224-efd12d52-c0ad-4e3e-a701-60b69854d9e0.PNG) <br/>
op code(01): CPU가 두 개의 수를 덧셈하라는 명령<br/>
ModRm:연산의 대상이 되는 operand(피연산자)를 표시<br/>
11 - 두 개의 덧셈 연산에 대한 operand가 모두 register임을 나타냄.<br/>
000, 011 - operand 지칭
- Assembly language<br/>
     _add eax, ebx_ <br/>
     add - mnemonic symbol 기억하고 연산하기 쉬운 단어 <br/>

어셈블리 명령은 하나의 머신 명령을 프로그래머가 다루기 쉬운 형태로 1:1 대응해서 표현한 형태

# The four field format 어셈블리 프로그램의 문법
- Assembly language program consists of lines <br/>
프로그램 한 줄에 어셈블리 명령을 하나씩 적음.<br/>
하나씩 적어놓은 것을 assembly statement, 어셈블리 문장이라고 함. <br/>
- Every statement in a line consists of four fields <br/>
한 줄의 어셈블리 문장에는 네 개의 필드가 올 수 있음.<br/>
어셈블리 문장 하나를 여러줄에 걸쳐서 쓸 수도 있음. - 이때 continuation 문자 사용 (백슬래쉬) <br/>
상황에 따라서 생략이 가능한 (optional) 사항들.<br/>
    - label field 생략가능
    - mnemonic (opcode) field 생략가능
    - operand field <br/>
    mnemonic field 존재여부, 어떤 mnemonic인지에 따라서 operand가 추가될 것인지 아닌지가 결정됨. 
    - comment field 생략가능 

![ssw_ch4_2](https://user-images.githubusercontent.com/63347989/137324037-12034da9-b033-4ad7-a15a-e5a2c0d69fa5.PNG)<br/>
공백문자의 제약은 없음 <br/>
보통 label이 한 줄의 맨 첫번째 열에서 시작을 하는데, label이전에도 공백문자가 있을 수 있음.<br/>
label 직후에는 보통 :(콜론)을 붙이기도 하고 생략할 수도 있음. <br/>
콜론문자가 사용되는 경우, 어디까지가 label인지 명확히 구분이 되기 때문에 :과 opcode간의 공백문자가 없을 수도 있음.<br/>
## 1. Label field <br/>
- 어셈블리 문장의 맨 앞에 놓임. <br/>
- 프로그램 내에서 해당 label이 붙어있는 곳곳의 메모리 주소를 특정한 symbol로 표현한 것. <br/>
- specifies the target of a jump instruction  <br/>
jump 명령의 타겟  <br/>
- jump is the same as goto  <br/>
jump 명령: 특정한 지점, 특정한 주소로 분기하는 명령 <br/>
분기 대상을 label로 삼을 수 있음.
- label을 구성하는 문자는 알파벳 문자, 숫자, _ , &#36; , #, ... <br/>
첫번째 글자는 알파벳, ., _, ? <br/>
. 으로 시작하는 label은 특별한 의미를 갖기 때문에 주의해야함.  <br/>
&#36; 로 시작하는 label은 identifier (reserved word 가 아님) <br/>
ex. EAX는 operand로 사용되었기 때문에 reserved word임. EAX를 쓰고 싶으면 &#36; EAX를 쓰면됨
- label의 길이는 4095자까지 가능


## 2. Mnemonic (opcode) field 
- an instruction specifier  <br/>
어떤 machine instruction을 나타내는지
- MOV, ADD, SUB, etc  <br/>
- the word mnemonic suggests that it makes the machine code easy to remember
- instruction에 따라서 앞에 prefix가 붙기도함
- psuedo instruction: CPU가 실제로 실행하는 instruction이 아닌 어셈블러가 처리하는 가짜 instruction

## 3. Operand field 
- objects on which the instruction is operating   <br/>
instruction이 동작하는 대상을 표시
- Each instruction have a fixed number of operands (0 ~ 3) <br/>
각 instruction 마다 정해진 operand의 개수가 있음
    - ADD takes two operands, JMP takes one operand, … 
    - x86 CPU의 경우에는 operand가 없는 경우부터 최대 3개까지를 가질 수 있음
- if there is more than one operand(복수개), they are separated by __commas__ <br/>
- Operands can have the following types: __register, memory, immediate, implied__  <br/>
operand의 유형 = insturction이 동작하는 대상의 유형<br/>
    - register, memory - 피연산자가 위치하는 곳
    - immediate - 명령의 상수를 피연산자 
    - implied - 명령 자체에는 명시적으로 피연산자가 표시되어있지 않은데 opcode가 뭔지에 따라서 피연산자가 미리 지정되어있는 경우

## 4. Comment field 주석
- contains documentation 
- It begins with semicolon <br/>
어셈블러가 프로그램을 번역할 때 한 줄에서 세미콜론이 나온 지점부터 해당 라인의 끝까지는 번역을 하는 과정에서 어셈블러가 번역을 하지 않음.
- documentation is especially important in assembly language, because it is hard to read 
- A line may consist of nothing but a comment

## Example program
어셈블리 프로그램의 형식만 중점적으로 살펴봄 <br/>

![ssw_ch4_3](https://user-images.githubusercontent.com/63347989/137347308-5d26cfc6-ddd7-4d50-9349-b1c33c8ea029.PNG) <br/>
ORD - label <br/>
SUB - mnemonic <br/>
EDX, EAX - operand <br/>
;--- - comment<br/>

label은 프로그램의 구조상, 기능상 필요한 경우에만 label을 붙이면 됨.<br/>
필요가 없는 곳 (대부분의 경우) 에는 굳이 label을 붙이지 않아도 됨. <br/>
comment만으로 이루어진 line도 존재함. 